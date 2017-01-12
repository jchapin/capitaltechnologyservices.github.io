---
layout: post
title: "Rails Asset Pipeline Issue in Google App Engine Flexible Environment"
title_short: "Rails Asset Issue in GAE Flex Environment"
author: "Chapin, John C."
excerpt: >
  We attempted a deployment of a Ruby on Rails application to Google App
  Engine's environment yesterday (2017-01-12) and the links to all of our assets
  broke. In short, roll back the version of Google's Cloud Components and it
  will probably fix your links.
---

{{ page.excerpt }}

## The Problem - No Fingerprints on Assets

At the end of most deployments we will run ```gcloud components update``` when
Google prompts that there's a new release of their Cloud SDK. Who doesn't? We
all want to make sure we have access to the cool new feature's in Google's Cloud
SDK.

We deployed a client application after beginning of the new year, a week went
by and we were ready for the next deployment. We pushed a new version up and
all of the links to Javascript, CSS, and images were broken. After combing
through the source code changes and looking for a commit that broke our asset
pipeline configuration, we turned up nothing. Our links still looked like this:

    /stylesheets/application.css

instead of this:

    /stylesheets/application-cb6e77b89d7e2f628faf475f1ff924935e65036aec2a332d16f228aa687f23c7.css

## The Solution - Restore Cloud SDK Components

When we confirmed there was nothing wrong with our application's configuration
we decided to roll back absolutely everything to be in line with our last
deployment, which meant running:

    gcloud components restore

This restored us to:

    Google Cloud SDK 135.0.0
    bq 2.0.24
    bq-nix 2.0.24
    core 2016.11.11
    core-nix 2016.11.07
    gcloud 
    gsutil 4.22
    gsutil-nix 4.18
    kubectl 
    kubectl-darwin-x86_64 1.4.4

After running another deployment we confirmed that all of our assets and their
fingerprints were back and we could move forward. We don't know the exact cause
for the problem yet. As of this morning the latest available Google Cloud SDK is
139.0.0.

    Google Cloud SDK 139.0.0
    bq 2.0.24
    bq-nix 2.0.24
    core 2017.01.11
    core-nix 2016.11.07
    gcloud 
    gsutil 4.22
    gsutil-nix 4.18
    kubectl 
    kubectl-darwin-x86_64 1.5.1

## A Note on Deployment

A moment to emphasize something important that you should take into account when
deploying to GAE. Don't just deploy with the default options to automatically
promote the new version and stop the previous version. Do this:

    gcloud app deploy --no-promote --no-stop-previous-version

After your application has been deployed, take the opportunity to confirm that
the new deployment is functional. Then you can use Google's web based console
to migrate traffic over to the new release when you know everything is working.
