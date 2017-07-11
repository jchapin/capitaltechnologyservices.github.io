---
layout: post
title: "Checklist - Continuous Integration Setup for a Rails 5 App"
title_short: "Checklist - CI Setup for Rails 5"
author: "Chapin, John C."
excerpt: >
  A checklist of all of the steps necessary to setup continuous integration for
  a new Rails 5 application. This list may not cover the specific details of
  configuring Jenkins, since that has already been accomplished. This checklist
  just serves as documentation of what needs to be done when we spin up a new
  project.
---

{{ page.excerpt }}

<img class="img-responsive" src="{{ page.image }}">

1.  Setup Development, Test, and Staging Environments

    1.  Change directory into the project's root directory.

    1.  ```heroku create app-name-dev --remote dev```

    1.  ```heroku create app-name-test --remote test```

    1.  ```heroku create app-name-staging --remote staging```

1.  On Jenkins, create a "New Item" cloned from one of the existing Rails 5
    Projects

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

1.  Install the pre-push hook from another Rails project

    1.  ```.git/hooks/pre-push```
