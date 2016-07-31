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

Version 1.35 of the plugin provides two places to control the maximum number of instances the plugin will allow to be run at one time.

The first controls how many EC2 instances the Amazon EC2 plugin is allowed to spin up altogether.
This value is set here:
Jenkins > Manage Jenkins > Configure System > Cloud > Amazon EC2 > Advanced > Instance Cap
We will refer to this value as the *EC2 Instance Cap*.

The other value controls the maximum number of EC2 instances of a particular type which can be run at once.
This value is set here:
Jenkins > Manage Jenkins > Configure System > Cloud > Amazon EC2 > (Node Type) > Advanced > Instance Cap
We will refer to this value as the *Node Type Instance Cap*.

Note that the EC2 Instance Cap has precedence over the *Node Type Instance Cap*,
so Node Type Instance Cap is expected to be less than or equal to EC2 Instance Cap.

Additionally, the Number of Executors per Node can be controlled via
Jenkins > Manage Jenkins > Configure System > Cloud > Amazon EC2 > (Instance) > Number of Executors ,
for our setup we will keep that value set to '1', i.e., run no more than 1 job per Node of this type.
Note that the Number of Executors value is independent of the Instance Caps discussed above.

### Observations

Here are some different settings for these values, and what I observed happening.

EC2 IC     | NodeType IC | Result
---------- | ----------- | ------
1          | 1           | 1 node is spawned, work is sent to it, jobs queued up waiting for this node
1          | 2           |
1          | 3           |
2          | 1           |
2          | 2           |
2          | 3           |
3          | 1           |
3          | 2           |
3          | 3           |
"" (blank) | "" (blank)  |
"" (blank) | 1           | (need to verify - think I saw EC2 IC = blank => no instance cap! :(
"" (blank) | 2           |
"" (blank) | 3           |

## References

[https://wiki.jenkins-ci.org/display/JENKINS/Amazon+EC2+Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Amazon+EC2+Plugin)
 
[http://www.cakesolutions.net/teamblogs/jenkins-and-on-demand-slaves-in-aws](http://www.cakesolutions.net/teamblogs/jenkins-and-on-demand-slaves-in-aws)
