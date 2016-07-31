---
layout: post
title: Notes on Jenkins' Amazon EC2 plugin
---

Some working notes on configuring & using Jenkins' Amazon EC2 plugin

[//]: # ( we are using this comment style )
[//]: # ( ![_config.yml]({{ site.baseurl }}/images/config.png) )

Using Amazon EC2 plugin 1.35, I noticed it spawns more slave nodes than I was expecting.
I would like to be able to control this, so I am investigating this.
Here are some notes on what I have found so far.

[//]: # (The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) )

This version of the plugin provides two places to control 

=== References ===

https://wiki.jenkins-ci.org/display/JENKINS/Amazon+EC2+Plugin
 
http://www.cakesolutions.net/teamblogs/jenkins-and-on-demand-slaves-in-aws
