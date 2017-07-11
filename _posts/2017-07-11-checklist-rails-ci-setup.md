---
layout: post
title: "Checklist - CI Setup for Rails 5"
title_short: "Checklist - CI Setup for Rails 5"
author: "Chapin, John C."
excerpt: >
  A checklist of all of the steps necessary to setup continuous integration for
  a new Rails 5 application. This list does not cover the specific details of
  configuring Jenkins, since that has already been accomplished. This checklist
  just serves as documentation of what needs to be done when we spin up a new
  Rails 5 project.
---

{{ page.excerpt }}

<img class="img-responsive" src="{{ page.image }}">

1.  Setup Development, Test, and Staging Environments

    Many of our projects start their life on Heroku with development, test, and
    staging environments being hosted on that platform. The dev environment is
    for very experimental ideas and troubleshooting and is occasionally deployed
    to manually. Test environments are used by internal QA teams. Staging
    environments are used for client user acceptance testing before a
    production release. The CI server (Jenkins) is setup to deploy to these
    environments whenever master is updated. So they need to be setup prior to
    configuring the project for CI. We can also host dev, test, and staging
    environments on Google Cloud Services (GCS) and Amazon Web Services (AWS).

    1.  Change directory into the project's root directory.

    1.  ```heroku create app-name-dev --remote dev```

    1.  ```heroku create app-name-test --remote test```

    1.  ```heroku create app-name-staging --remote staging```

1.  On Jenkins, create a "New Item" cloned from one of the existing Rails 5
    Projects

    All the Rails projects on Jenkins start out with a similar set of steps for
    running CI tasks, generating reports, and sending notifications. Consult
    the system administrator for which project to copy. You will then need to
    Configure the CI job to connect to the appropriate repository and deployment
    environments.

1.  Add Brakeman for Security Scanning

    1.  Create a new branch for this: ```brakeman-installation```

    1.  Locate ```group :development, :test do```

    1.  Add the following line: ```gem 'brakeman', require: false```

    1.  ```bundle install```

    1.  Commit Gemfile and Gemfile.lock

    1.  Submit this as a pull request.

1.  Add Rubocop for Static Code Analysis

    1.  Create a new branch for this: ```rubocop-and-config-file-installation```

    1.  Locate ```group :development, :test do```

    1.  Add the following lines:

        ```
        gem 'rubocop'
        gem 'rubocop-checkstyle_formatter', require: false
        ```

    1.  Create the configuration file, ```config/rubocop.yml```

        ```
        AllCops:
          Exclude:
            - 'db/**/*'
            - 'bin/*'
          TargetRubyVersion: 2.3
        ```

        Your ruby version may differ.

    1.  ```bundle install```

    1.  Commit Gemfile and Gemfile.lock

    1.  Submit this as a pull request.

1.  Add the CI Reporter Gem for JUnit Formatted Test Report Results

    1.  Add a new group in the Gemfile just for test environment only gems and
        include the ci_reporter_minitest gem.

        ```
        group :test do
          gem 'ci_reporter_minitest'
        end
        ```

    1.  Add a line to the ```.gitignore``` file to leave test reports out of the
        repository.

        ```
        # Ignore Test Reports
        test/reports
        ```

    1.  Add a block in the Rakefile to only require the ci_reporter's code to be
        used in development and test environments

        ```
        if ENV['RAILS_ENV'] == 'development' || ENV['RAILS_ENV'] == 'test'
          require 'ci/reporter/rake/minitest'
        end
        ```
        1.  ```bundle install```

        1.  Commit Gemfile, Gemfile.lock, Rakefile, and the .gitignore file.

        1.  Submit this as a pull request.

1.  Add SimpleCov for Test Coverage

    1.  Add ```gem 'simplecov', require: false``` to the test group of the
        Gemfile

    1.  Add a line to the ```.gitignore``` file to leave the coverage report out
        of the repository.

        ```
        # Ignore Test Coverage Report
        coverage
        ```

    1.  Add the following lines to the top of your test/test_helper.rb file,
        above any other lines with the exception of a pragma line such as
        ```# frozen_string_literal: true```

        ```
        require 'simplecov'
        SimpleCov.start 'rails'
        ```

    1.  ```bundle install```

    1.  Commit Gemfile, Gemfile.lock, test/test_helper.rb, and the .gitignore
        file.

    1.  Submit this as a pull request.

1.  Install optional tools that we do not run on Jenkins yet.

    1.  sass-lint

    1.  coffeelint

1.  Install a pre-push hook on your local repository.

    1.  ```.git/hooks/pre-push```

    1.  Customize the hook.

        ```
        #!/bin/sh
        # Brakeman - Security Scanner
        brakeman
        # Rubocop - Ruby Code Quality
        rubocop -c config/rubocop.yml -DR
        # CoffeeLint - CoffeeScript Lint Check
        coffeelint app/assets/javascripts/*.coffee
        # Sass Lint
        sass-lint --verbose --syntax scss
        # Tests
        bundle exec rake ci:setup:minitest test
        ```
