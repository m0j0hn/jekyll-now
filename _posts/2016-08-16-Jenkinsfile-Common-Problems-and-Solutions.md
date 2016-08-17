---
layout: post
title: Jenkinsfile Common Problems - and Solutions
---

Some working notes on configuring & using Jenkins' Jenkinsfile to build Github repository content

[//]: # ( we are using this comment style )
[//]: # ( ![_config.yml]({{ site.baseurl }}/images/config.png) )

Using Jenkins Jenkinsfile to build branches on my Github repo, and I have run into - and found solutions to - a number of problems.
Here are some notes on what I have found so far.

Problem: Branches with slash ('/') character in their name
Solution: Create new workspace (directory?) with %2F replaced by actual slash or underscore (?) TBD

Problem: Many branches > many workspaces > lots of disk space
Solution: deleteDir() after build is finished

Problem: Maven local repo defaults to ~/.m2/repository; simultaneous jobs interfere with each other
Solution: Pass Maven -Drepo.local=$workspace/.m2/repository setting, to locate Maven local repo in workspace dir
Bonus: Automatically removed when workspace is removed

Problem: blah-maven-plugin downloading files to ~/.m2/repository/blah directory; simultaneous jobs interfere with each other
Solution: Set download dir in pom.xml to use ${settings.localDirectory}/blah as download location.


## References

[https://wiki.jenkins-ci.org/display/JENKINS/Amazon+EC2+Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Amazon+EC2+Plugin)
 
[http://www.cakesolutions.net/teamblogs/jenkins-and-on-demand-slaves-in-aws](http://www.cakesolutions.net/teamblogs/jenkins-and-on-demand-slaves-in-aws)
