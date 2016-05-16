---
layout: post
title: 'Preparing a Ruby on Rails Application for Continuous Integration on Jenkins'
title_short: 'Preparing a Rails App for CI on Jenkins'
excerpt: "When we take on a new client, it is important that we begin monitoring product quality right away. In this post we discuss how we use continuous integration to monitor and improve our work."
---

Setting up continuous integration (CI) is an essential task whether we are
taking on a new client with a new project, or a new client with an, "old"
project. In brief, continuous integration allows us to run all of our quality
assurance tools on a product when a change to a product is proposed, accepted,
or even at just midnight on a random Tuesday. The latter case is more important
than you might think, but that's a subject for another post.

What we do want to concentrate on in this post is getting three basic metrics
on our Ruby on Rails project:

1.  The standards adherence of the written code to community standards.
1.  The pass / fail status of all the tests in the product's test suite(s).
1.  The amount of the product that is covered by the aforementioned test
    suite(s). That concept is referred to as, "code coverage".

This isn't everything that we evaluate in a good CI task or job, but it does
represent our minimum requirements to make any further changes to a project. We
don't make another move unless we know we're not breaking a previously
established expectation defined by a test, period.

## Code Quality with Rubocop

Since we use Jenkins and there's an excellent plugin for Jenkins to evaluate
CheckStyle XML files and report on code quality, we just need to convert the
output of Rubocop to CheckStyle XML.

Add the rubocop and rubocop-checkstyle_formatter to the development and test
groups of your application.

```
group :development, :test do
  gem 'rubocop'
  gem 'rubocop-checkstyle_formatter', require: false
  ...
```

Setup a configuration file: ```config/rubocop.yml``` to ignore a few important
directories:

```
AllCops:
  Exclude:
    - 'db/**/*'
    - 'bin/*'
  TargetRubyVersion: 2.3
```

Carve as few exceptions out as possible in this configuration file. Yes, it is
possible to ignore directories, ignore rules or, "cops", and cut down on the
number of complaints that you'll see. But if you make too many exceptions you
might one day find that you're ignoring an important signal about how your team
is writing their code. So ignore the automatically generated code of database
migrations and the scripts and utilities that ship with the framework. Don't
ignore the tests, the rake tasks, line length rules, method length rules, or
anything else you might write off as zealous. The more you read into why the
rules exist, you might just start to sympathize with them.

In Jenkins, you're going to have an opportunity to run a shell script with a
command or two defined, add the following to such a shell script.

```
bundle exec rubocop -c config/rubocop.yml --require rubocop/formatter/checkstyle_formatter --format RuboCop::Formatter::CheckstyleFormatter --no-color --rails --out tmp/checkstyle.xml || true
```

That's a really long one liner that writes a CheckStyle XML file into the
```/tmp``` directory of your project. If you have the CheckStyle plugin for
Jenkins installed, you should be able to add to your post-build tasks the
option to, "Publish CheckStyle analysis results". This will add a series of
reports and a very understandable graph to the build results page for your
CI job. In short, the graph should have a nearly flat or negative slope over
time as the quality of your code increases and the product develops.

