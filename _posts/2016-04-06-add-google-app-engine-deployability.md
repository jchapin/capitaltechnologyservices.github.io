---
layout: post
title: 'Transitioning a Rails App from Heroku to Google App Engine'
title_short: 'Transition from Heroku to GAE'
excerpt: "This article is a work in progress covering the basic steps of deploying a Rails app on Google App Engine."
---

One of our clients participated in a business accelerator program affiliated
with [Google for Entrepreneurs](https://www.googleforentrepreneurs.com). A
benefit afforded to our client was a generous hosting credit on the Google Cloud
Platform. In light of the client's growth and hosting costs the credit was
substantial enough that we made transitioning their project from Heroku to the
Google Cloud Platform a goal for their project.

This document will likely change. We started this work late in March of 2016 and
found that a lot of the documentation associated with Google's Services was out
of date or irrelevant to our particular goals. The Google Cloud Platform and
Google App Engine are constantly improving, and we are also enhancing the
deployment environment of our client on Google App Engine. So make sure to
revisit this document, send us notes, and feel free to ask questions.

1.  Install the Google Cloud SDK from https://cloud.google.com/sdk/
    1.  ```curl https://sdk.cloud.google.com | bash```
    1.  ```$ exec -l $SHELL``` or start up a new terminal
    1.  ```gcloud init``` This process will have you choose a login /
        configuration. I went ahead and tied the SDK to my regular account, then
        I went back and created a new configuration for the client's account.
1.  Create a configuration specfic to the client, who will be hosting the
    project.
    1.  ```gcloud config configurations create clientname```
    1.  ```gcloud config configurations activate NAMEOFYOURCLIENT```
    1.  ```gcloud init``` Choose to reinitialize this configuration with new
        settings, switch to your client's account.
1.  Setup app.yaml configuration file in the root of your Rails project. You
    can generate a configuration file for a custom project using the gcloud CLI
    by entering ```gcloud preview app gen-config --custom```

        runtime: custom
        vm: true
        health_check:
          enable_health_check: False
        entrypoint: foreman start web -p 8080
        resources:
          cpu: .5
          memory_gb: 1.3
          disk_size_gb: 10
        automatic_scaling:
          min_num_instances: 1
          max_num_instances: 2
          cool_down_period_sec: 60
          cpu_utilization:
            target_utilization: 0.5
        env_variables:
          PORT:                         8080
          ANOTHER_ENVIRONMENT_VAR:      value
          ...


    1.  The CLI tool give off a warning that the "preview app" command is in
        beta and backwards compatible changes may be introduced that effect your
        ability to manage app configurations they are currently. They also
        reference a URL that appears to lead to a bookmark that is no longer
        present in the documentation: https://cloud.google.com/sdk/#alternative
    1.  The runtime needs to be custom, because we're not going to be using the
        standard ruby environment from Google. If you don't specify "custom"
        then the gcloud deployment action will erase, create, and erase a
        standard Dockerfile during the deployment. Setting the runtime to custom
        means that YOU are going to be building the Dockerfile and gcloud
        shouldn't erase the file at anytime.
    1.  Set health checks to "False" at first. We're going to be implementing
        our own responses to the expected health checks that tell Google if the
        server / app is alive. If the health checks are not correctly
        implemented or turned off, servers will spin up and shut down in rapid
        succession. It makes sense when you think about it, the platform is
        spinning up an instance, deeming it unhealthy, and then trying to start
        a replacement.
    1.  The entrypoint specified here counts on the fact that you have specified
        a Procfile, which is expected on Heroku, and the GAE systems will start
        the application in a similar fashion on GAE.
    1.  Resources and automatic scaling are currently set to the defaults from
        some of the GAE deocumentations and tutorials. Our client's application
        fit inside of these parameters, so we have altered nothing at the
        moment.
    1.  Environment variables are important to the app booting up correctly. If
        you go back and look at your server configuration file
        (i.e. config/puma.rb) you'll likely see that the PORT is drawn from the
        app environment or otherwise defaults to 3000. GAE expects your app to
        be served on 8080, so the first environment variable you should setup is
        ```PORT: 8080```. After this, you can drop in most of the configuration
        settings from ```heroku config --app=YOURAPPNAME```. You're probably
        going to want to setup new services for your database, file storage, and
        who knows what else that you've rolled into your app on Heroku.
        Technically, you could connect to your Heroku PostgreSQL instances,
        Redis and other services on your Heroku account. But don't cut off those
        services or your Heroku account before transitioning away from them.
      
1.  Create a Dockerfile in the root of your project.

        # This Dockerfile for a Ruby application was generated by gcloud.

        # The base Dockerfile installs:
        # * A number of packages needed by the Ruby runtime and by gems
        #   commonly used in Ruby web apps (such as libsqlite3)
        # * A recent version of NodeJS
        # * A recent version of the standard Ruby runtime to use by default
        # * The bundler and foreman gems
        FROM gcr.io/google_appengine/ruby

        # Install ruby 2.2.3 if not already preinstalled by the base image
        RUN cd /rbenv/plugins/ruby-build && \
            git pull && \
            rbenv install -s 2.2.3 && \
            rbenv global 2.2.3 && \
            gem install -q --no-rdoc --no-ri bundler --version 1.11.2 && \
            gem install -q --no-rdoc --no-ri foreman --version 0.78.0
        ENV RBENV_VERSION 2.2.3

        # To install additional packages needed by your gems, uncomment
        # the "RUN apt-get update" and "RUN apt-get install" lines below
        # and specify your packages.
        # RUN apt-get update
        # RUN apt-get install -y -q (your packages here)

        # Install required gems.
        COPY Gemfile Gemfile.lock /app/
        RUN bundle install --deployment --without development test && rbenv rehash

        # Precompile Assets
        # RUN bundle exec rake assets:precompile

        # Start application on port 8080.
        COPY . /app/
        ENTRYPOINT foreman start web -p 8080

    1.  This Dockerfile is mostly stock. But one important detail to note is
        that you're going to want to specify installation of dependencies
        without development or test group gems. Otherwise, you might have to
        install a host of operating system dependencies you don't need. For
        example, installing QT in order to run testing framework tools.
1.  Set ```config.force_ssl = false``` in the Rails app's production environment
    configuration. Yes!, set this to false, because Google Cloud Platform is
    going to setup proxy servers in front of your application and manage the
    transition from HTTP to HTTPS. Keeping this set to true will just keep the
    proxy servers from being able to communicate with the application servers.
1.  Verify your domain with GAE / Google Cloud Platform.
    1.  This is one of the most tedious tasks, as Google's UI kept saying that
        I needed to verify ownership of the domain both before and after adding
        the domain and uploading the SSL certificate. Changes to settings seem
        to be very slow to take hold and may even conflict with trying to verify
        the domain's ownership for other services and purposes, like Google
        Analytics.  
1.  Installing an SSL Certificate
    1.  It's should be possible to use the gcloud CLI to accomplish this task
        but I performed these steps through the web based interface, because SSL
        certificates added through the CLI were not showing up in the developer
        after using the CLI.
    1.  ```gcloud compute ssl-certificates create clientdomain --certificate wildcard.clientdomain.com.crt --private-key wildcard.clientdomain.com.key```
    1.  Note that the resource name after create has to fit the following
        regular expression: ```(?:[a-z](?:[-a-z0-9]{0,61}[a-z0-9])?)```,
        That means it has to start with a lowercase letter, feature lowercase
        letters or numbers up to a length of 62 characters and then end with a
        lowercase letter or number. That's a total length of 64 characters max.
    1.  Switch the format of the key file to PEM:
        ```openssl rsa -in host.domain.com.key -out host.domain.com.key.pem```
1.  Deploy the application: ```gcloud preview app deploy```
    1.  After this step, you should be able to visit the app at:
        http://projectname-1234.appspot.com

Things that might trip you up / solutions
=========================================

1.  Given the rapid changes to the Google Cloud Platform, you might want to
    review their release notes before and during a deployment.
    https://cloud.google.com/sdk/release_notes
1.  After installing the Google Cloud SDK, I had to restart my system for an app
    configuration to be generated correctly. Otherwise, the software just idled
    for minutes without action.
1.  We originally started with by looking at Google's, ["Hello World"](https://cloud.google.com/ruby/getting-started/hello-world#running_hello_world)
    exercise for Ruby Apps. We had to toss most of these directions as they were
    a bit dated and research after the gcloud client malfunctions lead us to
    the conclusion that we needed a custom environment instead of a typical ruby
    environment.

    