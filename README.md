cyg
=======

cyg is a command-line installer for [Cygwin](http://cygwin.com/) which cooperates with Cygwin Setup and uses the same repository. 
This project is based on *kou1okada/apt-cyg*  

The syntax is similar to apt-get. Usage examples:

* "cyg install &lt;package names&gt;" to install packages
* "cyg remove &lt;package names&gt;" to remove packages
* "cyg update" to update setup.ini
* "cyg show" to show installed packages
* "cyg find &lt;pattern(s)&gt;" to find packages matching patterns
* "cyg describe &lt;pattern(s)&gt;" to describe packages matching patterns
* "cyg packageof &lt;commands or files&gt;" to locate parent packages
* "cyg pathof &lt;cache|mirror|mirrordir|cache/mirrordir|setup.ini&gt;" to show path
* "cyg key-add &lt;files&gt; ..." to add keys contained in &lt;files&gt;
* "cyg key-del &lt;keyids&gt; ..." to remove keys &lt;keyids&gt;
* "cyg key-list" to list keys
* "cyg key-finger" to list fingerprints
* "cyg upgrade-self" to upgrade apt-cyg
* "cyg depends &lt;package names&gt;" to show forward dependency information for packages with depth.
* "cyg rdepends &lt;package names&gt;" to show reverse dependency information for packages with depth.
* "cyg completion-install" to install completion.
* "cyg completion-uninstall" to uninstall completion.
* "cyg mirrors-list" to show list of mirrors.
* "cyg benchmark-mirrors &lt;url&gt; ..." to benchmark mirrors.
* "cyg benchmark-parallel-mirrors &lt;url&gt; ..." to benchmark mirrors in parallel.
* "cyg benchmark-parallel-mirrors-list" to benchmark mirrors-list in parallel.
* "cyg scriptinfo" to show script infomations.
* "cyg show-packages-busyness &lt;package names&gt; ..." to show packages are busy or noe.
* "cyg dist-upgrade" to upgrade all packages that is installed. This subcommand uses setup-*.exe
* "cyg update-setup" to update setup.exe
* "cyg setup" to call setup.exe
* "cyg packages-total-count" count number of total packages from setup.ini
* "cyg packages-total-size" count size of total packages from setup.ini
* "cyg packages-cached-count" count number of cached packages in cache/mirrordir.
* "cyg packages-cached-size" count size of cached packages in cache/mirrordir.

Requirements
------------

cyg requires the cygwin default environment and optional packages below.

* wget,ca-certificates,gnupg

Quick start
-----------

 [ TODO ]

To use cyg, for example:

    cyg install nano

Features
------------

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
