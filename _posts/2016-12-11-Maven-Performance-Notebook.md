---
layout: post
title: Maven Performance Notebook
category: Dev
tags: [java,maven]
---

Some working notes on configuring & using Maven for best performance

[//]: # ( we are using this comment style )
[//]: # ( ![_config.yml]({{ site.baseurl }}/images/config.png) )

Using Maven to build our code I have run into - and found solutions to - a number of problems.
Here are some notes on what I have found so far.


## Use compiler:compile for iterative compiles

### Problem: Maven "compile" phase is slow, because it runs all preceding lifecycle events

### While correct, this often results in Maven doing more work than necessary, especially for developers building frequent changes in their sandbox

### Solution: mvn compiler:compile goal

### Discussion: Not a replacement for `compile`, but a useful tool for rapid development before running `mvn compile`

```
mvn compiler:compile
```


## Use `-o` (offline) to use previously downloaded artifacts from m2 cache

### Problem: Builds can "download the Internet" to satisfy dependencies before building.

### Solution: Use `-o` option to only use what was previously downloaded to cache

### Discussion: Depends on previous build successfully downloading needed dependencies.

```
mvn -o ...
```


## References

[https://wiki.jenkins-ci.org/display/JENKINS/Plugin+tutorial](https://wiki.jenkins-ci.org/display/JENKINS/Plugin+tutorial) Recommendation to use `compiler:compile`

[https://wiki.jenkins-ci.org/display/JENKINS/Plugin+tutorial](https://wiki.jenkins-ci.org/display/JENKINS/Plugin+tutorial) Recommendation to use `-o`
 
