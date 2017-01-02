---
layout: post
title: "2017 Resolutions - 2. Measure Site Performance"
title_short: "Measure Site Performance"
excerpt: >
  If your site is slow to load you're losing mobile market share and better
  placement in search engine results.
---

{{ page.excerpt }}

## You know the frustration

You've been there, trying to load your website during a presentation or sharing
a link with a lead while at their location, and the page... just... won't...
finish... loading. Maybe you tell the audience to try loading the site at work.
Maybe it's your mobile phone provider's problem, they're always slow in this
part of town. Maybe your computer is getting old and you need a new one.

## Or maybe your site's hosting isn't configured properly

  * Your front page assets total several megabytes in uncompressed images
    with an unnecessarily large color palette
  * The JavaScript, CSS, and markup for the site isn't compressed and "minified"
  * There are no cache control directives on your site, so browsers request
    assets that they don't need to update
  * Your web server responds, "slowly"

How on earth would you ever know any of the above? By consulting the following
tools right now:

  * <a href="https://developers.google.com/speed/pagespeed/insights/" target="_blank">Google PageSpeed Insights</a>
  * <a href="https://tools.pingdom.com" target="_blank">Pingdom Website Speed Test</a>

## Eek! I got a 50/100!

This isn't surprising, but it probably isn't hard to fix either. The most common
error that we see is that the dimensions of images are considerably larger than
they need to be. On one company's staff page, the images were sixteen times the
size that they would ever be rendered for a desktop browser. Complaints about
compression, minification, and cache control headers usually require a quick
tweak to your web server's configuration files.

## I got a 75/100 and I have never been a "C" student!

Relax a bit. These tests are very tough to get a perfect score on and were
engineered as a measuring stick for sites with hundreds of thousands of daily
visitors. Just because you got an "average" grade doesn't mean that you're
doing poorly. But what you probably should do is check the PageSpeed Insight
scores of your competitors. Search engines take into account how quickly a site
loads as well as the relevance of the content. So with all other things being
equal, you want to make sure you're at least a little faster than your
competition.

## Think about mobile clients

In your home and office, you probably have a broadband connection. The Google
Pagespeed Insights and Pingdom tests are located in data centers with even
better connectivity than your home or office. Now consider that mobile bandwidth
varies based on multiple factors such as wireless network coverage, network
congestion, and physical obstacles to radio signals. If your business is
reliant on customers accessing your sites with mobile devices, you're going to
need to be extra vigilant about performance.

## Set a standard

We aim for initial page load times of under 1.5 seconds in all of the Rails
applications that we build. That can be an ambitious target on media rich
websites, but there are strategies for achieving fast load times beyond basic
server configuration tweaks.

<a href="mailto:sales@cts-llc.net?subject=Improve My Site Performance">Contact us to improve your website's performance.</a>
