# cyg: Cygwin Package Manager

cyg is a command-line installer for [Cygwin](http://cygwin.com/) which cooperates with Cygwin Setup and uses the same repository. 
This project is based on *kou1okada/apt-cyg*  

## Requirements

cyg requires the cygwin default environment and optional packages below.

* aria2,ca-certificates,gnupg

## Quick start

To use cyg, for example:

    cyg install nano

## A Note About Licensing
```
This fork is released under GPLv3 License
You may copy, distribute and modify the software as long as you track changes/dates in source files.
Any modifications to or software including (via compiler) GPL-licensed code must also be made available
under the GPL along with build & install instructions. ( see GPLv3.txt )
```
The main reason that this branch was born was about a sad DCMA about transcode-open/apt-cyg  
You can read full notice here: https://github.com/github/dmca/blob/master/2016-01-26-apt-cyg.md  
So i forked kou1okada/apt-cyg (MIT) and **legally sublicensed** it to **GPLv3** to avoid problems like this in the future.. 
  
## Features

### Faster downloads 
Thanks to **aria2c** multi part downloads help faster downloads. (Similar to apt-fast)

### dist-upgrade support

This fork has achieved dist-upgrade command by using `setup-x86.exe` and `setup-x96_64.exe` as a backend.
Note that all of running tasks on the cygwin will be killed before starting dist-upgrade.

### Multiple hash algorithms support

After the middle of 2015-03, the cygwin project changed the hash algorithm for checking tarball from md5 to sha512.
But, as of 2015-04-09, the cygwinports project seems still using md5.
This fork is available for both of cygwin and cygwinports by supporting algorithm of md5, sha1, sha224, sha256 and sha512.

### True multi-architecture support

Let think a case that you want to install the x86 package when you are working under the x86_64 environment.
For example:

    cyg --charch x86 install chere

As of 2013-10-26, chere package is provided for only the repository for x86.

Remarks:
Of course, you must install both environments of x86_64 and x86, beforehand.

### Signature check and key management by GnuPG

The default action of cyg has been changed to check signature for 'setup.ini'.
Of course you can also avoid signature check by using --no-verify or -X options.
Public keys of cygwin and cygwinports are already registered to trusted keys of embeded.
If you want to use some other public keys, please use key-* subcommands.

### Upgrade cyg

If cyg is under the git version control, this fork can upgrade itself by "upgrade-self" subcommand.
Therefore, the most recommended way to deploy this fork is below:

    git clone HTTPS_clone_URL
    ln -s "$(realpath cyg/apt-cyg)" /usr/local/bin/

`HTTPS_clone_URL` is like a `https://github.com/USERNAME/cyg.git`.
It can be found from the right side menu in each fork pages on github.
    
### Proxy support

Use --proxy, -p option.
This option must take a parameter from one of "auto", "inherit", "none" and URL.

* "auto" will determine a proxy using a part of the [Web Proxy Auto-Discovery Protocol (WPAD)](http://en.wikipedia.org/wiki/Web_Proxy_Autodiscovery_Protocol).
The current implementation will look for a string of "PROXY URL" from "http://wpad/wpad.dat".
If "wpad.dat" could not be downloaded, the proxy settings are inherited from the parent environment.
* "inherit" will inherit the proxy settings from the parent environment.
* "none" will not use the proxy.
* URL can take a string like "protocol://hostname:port".

For example:

    cyg --proxy http://proxy.home:8080 update

The default parameter is "${APT\_CYG\_PROXY:-auto}".
At the environment where is not provided the WPAD server, it makes the lag for a few seconds at startup.
So, if you don't want to use WPAD, please define APT\_CYG\_PROXY environment variable as below:

    export APT_CYG_PROXY=inherit

### Bash completion support

Bash completion script can be installed  to "/etc/bash_completion.d/cyg" by `completion-install` subcommand.
It will be automatically updated when cyg is upgraded to newer version.
If you don't want to update it automatically, execute `completion-install` subcommand in conjunction with `--completion-disable-autoupdate` option. And `completion-uninstall` subcommand removes "/etc/bash_completion.d/cyg".
