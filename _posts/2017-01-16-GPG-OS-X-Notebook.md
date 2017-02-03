---
layout: post
title: GPG macOS Notebook
category: Dev
tags: [aws,ci,github,jenkins,sqs]
---

Working notes on installing and using GPG on OS X

[//]: # ( we are using this comment style )
[//]: # ( ![_config.yml]({{ site.baseurl }}/images/config.png) )

Background: We would like to encrypt and decrypt files using GPG on macOS (OS X).
The GNU Privacy Guard [https://www.gnupg.org/](GPG) is a GNU implementation of Phil Zimmerman's Pretty Good Privacy [https://en.wikipedia.org/wiki/Pretty_Good_Privacy](PGP).
Both toolchains enable users to generate and manage crytpographic keys, and to encrypt and crytographically sign files.
The theory of these tools is beyond the scope of this document; please see the references for more information.
This document simply provides my notes on installing and using GPG so others can easily do this, too.

Prerequisites: macOS (OS X), Homebrew, Admin (root) access (necessary for installing Homebrew software)

## Update Homebrew

From a shell prompt, update Homebrew:

    $ brew update

    Updated 1 tap (caskroom/cask).

## Install GPG

From shell prompt, install GPG:

    $ brew install gpg

    ==> Installing dependencies for gnupg2: libgpg-error, libgcrypt, libksba, libassuan, pinentry, pth, gpg-agent, dirmngr, libusb, libusb-compat
    ==> Installing gnupg2 dependency: libgpg-error
    ==> Downloading https://homebrew.bintray.com/bottles/libgpg-error-1.26.sierra.bottle.tar.gz
    ######################################################################## 100.0%
    ==> Pouring libgpg-error-1.26.sierra.bottle.tar.gz
    🍺  /usr/local/Cellar/libgpg-error/1.26: 21 files, 479.2K
    ==> Installing gnupg2 dependency: libgcrypt
    ==> Downloading https://homebrew.bintray.com/bottles/libgcrypt-1.7.5.sierra.bottle.tar.gz
    ######################################################################## 100.0%
    ==> Pouring libgcrypt-1.7.5.sierra.bottle.tar.gz
    🍺  /usr/local/Cellar/libgcrypt/1.7.5: 19 files, 2.7M
    ==> Installing gnupg2 dependency: libksba
    ==> Downloading https://homebrew.bintray.com/bottles/libksba-1.3.5.sierra.bottle.tar.gz
    ######################################################################## 100.0%
    ==> Pouring libksba-1.3.5.sierra.bottle.tar.gz
    🍺  /usr/local/Cellar/libksba/1.3.5: 13 files, 359.4K
    ==> Installing gnupg2 dependency: libassuan
    ==> Downloading https://homebrew.bintray.com/bottles/libassuan-2.4.3_1.sierra.bottle.tar.gz
    ######################################################################## 100.0%
    ==> Pouring libassuan-2.4.3_1.sierra.bottle.tar.gz
    🍺  /usr/local/Cellar/libassuan/2.4.3_1: 14 files, 427K
    ==> Installing gnupg2 dependency: pinentry
    ==> Downloading https://homebrew.bintray.com/bottles/pinentry-0.9.7.sierra.bottle.1.tar.gz
    ######################################################################## 100.0%
    ==> Pouring pinentry-0.9.7.sierra.bottle.1.tar.gz
    🍺  /usr/local/Cellar/pinentry/0.9.7: 10 files, 168.3K
    ==> Installing gnupg2 dependency: pth
    ==> Downloading https://homebrew.bintray.com/bottles/pth-2.0.7.sierra.bottle.2.tar.gz
    ######################################################################## 100.0%
    ==> Pouring pth-2.0.7.sierra.bottle.2.tar.gz
    🍺  /usr/local/Cellar/pth/2.0.7: 16 files, 476.5K
    ==> Installing gnupg2 dependency: gpg-agent
    ==> Downloading https://homebrew.bintray.com/bottles/gpg-agent-2.0.30_1.sierra.bottle.tar.gz
    ######################################################################## 100.0%
    ==> Pouring gpg-agent-2.0.30_1.sierra.bottle.tar.gz
    ==> Caveats
    Remember to add "use-standard-socket" to your ~/.gnupg/gpg-agent.conf
    file.
    ==> Summary
    🍺  /usr/local/Cellar/gpg-agent/2.0.30_1: 11 files, 695.8K
    ==> Installing gnupg2 dependency: dirmngr
    ==> Downloading https://homebrew.bintray.com/bottles/dirmngr-1.1.1_2.sierra.bottle.tar.gz
    ######################################################################## 100.0%
    ==> Pouring dirmngr-1.1.1_2.sierra.bottle.tar.gz
    🍺  /usr/local/Cellar/dirmngr/1.1.1_2: 66 files, 527.7K
    ==> Installing gnupg2 dependency: libusb
    ==> Downloading https://homebrew.bintray.com/bottles/libusb-1.0.21.sierra.bottle.tar.gz
    ######################################################################## 100.0%
    ==> Pouring libusb-1.0.21.sierra.bottle.tar.gz
    🍺  /usr/local/Cellar/libusb/1.0.21: 29 files, 510.4K
    ==> Installing gnupg2 dependency: libusb-compat
    ==> Downloading https://homebrew.bintray.com/bottles/libusb-compat-0.1.5.sierra.bottle.1.tar.gz
    ######################################################################## 100.0%
    ==> Pouring libusb-compat-0.1.5.sierra.bottle.1.tar.gz
    🍺  /usr/local/Cellar/libusb-compat/0.1.5: 13 files, 93.5K
    ==> Installing gnupg2 
    ==> Downloading https://homebrew.bintray.com/bottles/gnupg2-2.0.30_3.sierra.bottle.tar.gz
    ######################################################################## 100.0%
    ==> Pouring gnupg2-2.0.30_3.sierra.bottle.tar.gz
    ==> Using the sandbox
    🍺  /usr/local/Cellar/gnupg2/2.0.30_3: 93 files, 3.7M

From shell prompt, confirm GPG is installed:

    $ which gpg

    /usr/local/bin/gpg

From shell prompt, ask GPG for its version:

    $ gpg -version

    gpg: directory `/Users/john/.gnupg' created
    gpg: new configuration file `/Users/john/.gnupg/gpg.conf' created
    gpg: WARNING: options in `/Users/john/.gnupg/gpg.conf' are not yet active during this run
    gpg: keyring `/Users/john/.gnupg/secring.gpg' created
    gpg: keyring `/Users/john/.gnupg/pubring.gpg' created
    gpg: sion: skipped: No public key
    gpg: [stdin]: encryption failed: No public key



## Generate a GPG key pair

[https://help.github.com/articles/generating-a-new-gpg-key](https://help.github.com/articles/generating-a-new-gpg-key)

From shell prompt, use GPG to generate a key pair:

    $ gpg --gen-key

    gpg (GnuPG) 2.0.30; Copyright (C) 2015 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    Please select what kind of key you want:
       (1) RSA and RSA (default)
       (2) DSA and Elgamal
       (3) DSA (sign only)
       (4) RSA (sign only)
    Your selection? 

From shell prompt, either specify the kind of key you want, or press Enter to accept the default:

    <Enter>

    RSA keys may be between 1024 and 4096 bits long.
    What keysize do you want? (2048) 

Enter the desired key size. We recommend the maximum key size of 4096.

    What keysize do you want? (2048) 4096

    Requested keysize is 4096 bits       
    Please specify how long the key should be valid.
             0 = key does not expire
          <n>  = key expires in n days
          <n>w = key expires in n weeks
          <n>m = key expires in n months
          <n>y = key expires in n years
    Key is valid for? (0) 

Enter the length of time the key should be valid. Press Enter to specify the default selection, indicating that the key doesn't expire.

    Key is valid for? (0) <Enter>

    Key does not expire at all
    Is this correct? (y/N) 

Confirm your selection by entering 'y'.

    Is this correct? (y/N) y
                        
    GnuPG needs to construct a user ID to identify your key.

    Real name: 

Enter a name that you would like associated with this keypair.

    Real name: John Doe

    Email address:

Enter an email address you would like associated with this keypair.

    Email address: user@example.com

    Comment:

Enter a comment to help identify this keypair.

    Comment: your message here

    You selected this USER-ID:     
    "John Doe (your message here) <user@example.com>"

    Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? 

Edit your settings or confirm your selections by pressing the O key.


TODO To Be Continued...

## References

[https://link_to_reference1](description of reference 1 - can be the URL, again)
 
[http://link_to_reference2](description of reference 2 - can be the URL, again)
