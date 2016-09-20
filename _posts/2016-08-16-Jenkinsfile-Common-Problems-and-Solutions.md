---
layout: post
title: Jenkinsfile Common Problems - and Solutions
---

Some working notes on configuring & using Jenkins' Jenkinsfile to build Github repository content

[//]: # ( we are using this comment style )
[//]: # ( ![_config.yml]({{ site.baseurl }}/images/config.png) )

Using Jenkins Jenkinsfile to build branches on my Github repo, and I have run into - and found solutions to - a number of problems.
Here are some notes on what I have found so far.


## Use timeout( TIMEOUT_VALUE ){} to limit build durations

### Problem: Builds can take forever when users check in broken tests

### Solution: Time limit builds by using timeout()

### Discussion: Even if users commit changes which run forever - or longer than we are willing to wait - system can kill jobs and continue gracefully

```groovy
def TIMEOUT = 60 //minutes
timeout(TIMEOUT) {
  //do build stuff
}
```


## Use deleteDir() to remove workspaces after each build

### Problem: Lots of branches use lots of workspaces, which uses lots of disk space

### Solution: Reclaim disk space by using deleteDir() to remove workspaces after each build.

### Discussion: It turns out we most often use the data that Jenkins keeps on master, and very seldom want to examine the workspace itself, so there is usually no reason to keep the workspaces around, so by default we use deleteDir() to remove workspaces as soon as job is finished.

```groovy
if (! env.NO_DELETE_WORKSPACE) {
  dir(workspace) {
    deleteDir()
    echo "Removed workspace directory: " + pwd()
  }
}
```


## Use workspace-local Maven repositories to isolate jobs dependencies

### Problem: Since Maven local repository defaults to ~/.m2/repository simultaneous jobs on same worker node can interfere with each other since this is a shared directory

### Solution: By passing Maven the -Drepo.local=$workspace/.m2/repository setting, we can locate the Maven local repository in the workspace directory.
Also, by locating the Maven local repository in the workspace, it will be cleaned up when the workspace is removed.

```groovy
PWD = pwd()

def mvnHome = tool 'maven-3.2.1'
def mvn = "${mvnHome}/bin/mvn"

def MAVEN_REPO_LOCAL = env.MAVEN_REPO_LOCAL ? env.MAVEN_REPO_LOCAL : "-Dmaven.repo.local=${PWD}/.m2/repository"

sh "$mvn clean install $MAVEN_REPO_LOCAL"
```


## Isolate job downloads by configuring maven plugins to download to workspace

### Problem: blah-maven-plugin downloads files to ~/.m2/repository/blah directory; concurrent jobs interfere with each other

### Solution: Set download dir in pom.xml to use ${settings.localDirectory}/blah as download location.


## Special character '/' in workspace name confuses some tools

### Problem: Branches with slash ('/') character in their name, even mapped to %2F, confuse some tools

### Solution: Create new workspace with "%2F" replaced by the underscore character.

### Discussion: avro-maven-plugin still has problem with this, for example.

```groovy
//See: https://issues.jenkins-ci.org/browse/JENKINS-30744
def workspace = pwd().replace("%2F", '_')

ws(workspace) {
  // stuff
}
```


## Special character '@' in workspace name can confuse some tools

### Problem: Concurrent builds on the same node result in workspaces with '@' character in workspace name, e.g., workspace@2, workspace@3, etc., which confuses Avro somehow.

### Solution: Change Jenkins' workspace list separator from default '@' to some other character, like '-' (hyphen)

### Example: java -Dhudson.slaves.WorkspaceList=- -jar jenkins.war

### Discussion: A Java system property can be used to set the Jenkins workspace list separator: -Dhudson.slaves.WorkspaceList=SEPARATOR.
I added this setting to my /etc/sysconfig/jenkins script, which is used by the /etc/init.d/jenkins run script.

Failure looks like this, for example:

```shell
testFilterSyslogHostPrefix(com.example.transform.TestProvidedTransformations) Time elapsed: 0.069 sec  <<< ERROR!
java.lang.IllegalStateException: Schema file '/jenkins/workspace/EXAMPLE/example/EXAMPLE-1234%402/example-core/target/test-classes/avro-schemas/flattened-syslog.avsc' does not exist
    at com.example.transform.conf.AvroRecordBuilderOperationParser.parse(AvroRecordBuilderOperationParser.java:53)
    at com.example.transform.conf.MapOperationParser.parse(MapOperationParser.java:58)
    at com.example.transform.conf.OperationConfig.parse(OperationConfig.java:108)
    at com.example.transform.TestProvidedTransformations.testFilterSyslogHostPrefix(TestProvidedTransformations.java:40)
```

[https://issues.jenkins-ci.org/browse/JENKINS-25242](https://issues.jenkins-ci.org/browse/JENKINS-25242)


## References

[https://wiki.jenkins-ci.org/display/JENKINS/Amazon+EC2+Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Amazon+EC2+Plugin)
 
[http://www.cakesolutions.net/teamblogs/jenkins-and-on-demand-slaves-in-aws](http://www.cakesolutions.net/teamblogs/jenkins-and-on-demand-slaves-in-aws)
