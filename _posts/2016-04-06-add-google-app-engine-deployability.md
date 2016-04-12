Transition from Heroku to Google App Engine

1.  Install the Google Cloud SDK from https://cloud.google.com/sdk/
    1.  ```curl https://sdk.cloud.google.com | bash```
    1.  ```$ exec -l $SHELL``` or start up a new terminal
    1.  ```gcloud init``` This process will have you choose a login /
        configuration. I went ahead and tied the SDK to my regular account, then
        I went back and created a new configuration for the client's account.
1.  Create a configuration specfic to the client, who will be hosting the
    project.
    1.  ```gcloud config configurations create clientname```
    1.  ```gcloud config configurations activate milligram```
    1.  ```gcloud init``` Choose to reinitialize this configuration with new
        settings, switch to your client's account.
1.  Proceed, referring to the instructions here:
    https://cloud.google.com/ruby/getting-started/hello-world#running_hello_world
1.  I had to toss out most of Google's directions to get up and running due to
    a problem with their CLI tool during the Dockerfile generation step. During
    this period of time I ended up installing kubernetes via gcloud and Docker
    via brew. I've had issues in the past where Google's documentation often
    assumes you have certain dependencies installed, and you only find out about
    that after digging into a StackOverflow post or two. So you might want to:
    1.  Install kubectl: ```gcloud components update kubectl```. It will prompt
        you that you need to install it if it hasn't been installed before.
    1.  Install Docker: ```brew install docker```
1.  Restart. Ultimately I just restarted my system, came back and ran the rest
    of these instructions, and the app config generation proceeded without
    error. If someone is reading this and can confirm that steps in my process
    can be eliminated, let us know.
1.  Another problem is that Google App Engine deletes its own failed Dockerfile
    for custom images when a deployment doesn't succeed. During one of the
    deployments I copied and saved the Dockerfile for alteration and inclusion
    on the next deployment.
1.  Counter to some of the indications that the Ruby tutorial and other guidance
    sites give. You want to set your runtime to "custom". That's the only way
    that a custom Dockerfile will be respected on deployement. Otherwise the
    gcloud client creates and wipes out its own Dockerfile on each attempted
    deployment.

1.  Tweak the application configuration: https://github.com/GoogleCloudPlatform/getting-started-ruby


1.  Setup app.yaml configuration file in the root of your Rails project:

        runtime: ruby
        vm: true
        entrypoint: bundle exec rackup -p 8080 -E production config.ru
        resources:
          cpu: .5
          memory_gb: 1.3
          disk_size_gb: 10
        automatic_scaling:
          min_num_instances: 1
          max_num_instances: 5
          cool_down_period_sec: 60
          cpu_utilization:
            target_utilization: 0.5

1.  Create Dockerfile ```gcloud preview app gen-config --custom```
    1.  The CLI tool give off a warning that the "preview app" command is in
        beta and backwards compatible changes may be introduced that effect your
        ability to manage app configurations they are currently. They also
        reference a URL that appears to lead to a bookmark that is no longer
        present in the documentation: https://cloud.google.com/sdk/#alternative
    1.  This is probably because the documentation for their Ruby deployment
        routine is old and broken. After a few minutes of waiting around, that
        command did absolutely nothing...
    1.  Referring to Google Cloud's release notes at:
        https://cloud.google.com/sdk/release_notes
    1.  Tried running the command without the ```--custom``` flag and got:
        ```ERROR: (gcloud.preview.app.gen-config) Configuration file already exists. This command generates an app.yaml configured to run an application on Google App Engine. To create the configuration files needed to run this application with docker, try `gcloud preview app gen-config --custom`.```
    1.  So it must take a long time for it to generate a Dockerfile...

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
    1.  ```gcloud compute ssl-certificates create milligram --certificate wildcard.milligram.biz.crt --private-key wildcard.milligram.biz.key```
    1.  Note that the resource name after create has to fit the following
        regular expression: ```(?:[a-z](?:[-a-z0-9]{0,61}[a-z0-9])?)```,
        That means it has to start with a lowercase letter, feature lowercase
        letters or numbers up to a length of 62 characters and then end with a
        lowercase letter or number. That's a total length of 64 characters max.
    1.  Switch the format of the key file to PEM:
        ```openssl rsa -in host.domain.com.key -out host.domain.com.key.pem```
    