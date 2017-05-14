---
layout: post
title: Windows NTP Notebook
category: Dev
tags: [ntp,windows]
---

# Configure and run NTP on Windows

[//]: # ( we are using this comment style )
[//]: # ( ![_config.yml]({{ site.baseurl }}/images/config.png) )

## Background
By default, Windows ships with an NTP client named `w32tm`, but it may be misconfigured and not running by default, resulting in clock skew.

Windows ships with an NTP client named `w32tm`.
I have observed that it is sometimes not configured correctly, resulting in my Windows machines clocks being out of sync with the rest of my hosts.
This notebook contains procedure I used to to update w32tm configuration and restart the service.
This resulted in my machines clocks being in sync with the rest of my hosts.

## Prerequisites
Windows hosts with w32tm installed
Account on Windows hosts with Administrator privileges.

## Steps

1. Log into Windows box as someone who can become Administrator.
It is common to use some kind of Windows Remote Desktop (RDP) client.

2. Start an Administrator shell.

Powershell icon > Run as Administrator

The system will reply with something like:

```shell
Windows Powershell
Copyright (C) 2014 Microsoft Corporation. All rights reserved.
PS C:\ >
```
    
3. Enter the following command at the shell prompt:

```shell
w32tm /config /update /manualpeerlist:"0.pool.ntp.org,0x8 1.pool.ntp.org,0x8 2.pool.ntp.org,0x8 3.pool.ntp.org,0x8" /syncfromflags:MANUAL
```

The system will reply with:

```shell
The following error occurred: The service has not been started (0x80070426)
```

4. Enter the following command at the shell prompt:

```shell
w32tm /debug /disable
```

The system will simply return the shell prompt.

5. Enter the following command at the shell prompt:

```shell
w32tm /unregister
```

The system will reply with:

```shell
W32Time successfully unregistered
```

6. Enter the following command at the shell prompt:

```shell
w32tm /register
```

The system will reply with:

```shell
W32Time successfully registered
```

7. Enter the following command at the shell prompt:

```shell
net start w32time
```

The system will reply with:

```shell
The Windows Time service is starting.
The Windows Time service was started successfully
```

Congratulations! You have configured and started NTP on Windows!


## References

[http://support.ntp.org/bin/view/Support/WindowsTimeService](http://support.ntp.org/bin/view/Support/WindowsTimeService)

[https://www.sevenforums.com/general-discussion/2984-w32time-service-not-starting-automatically.html]([https://www.sevenforums.com/general-discussion/2984-w32time-service-not-starting-automatically.html)
