logstalgia-scripts
=================

Batch and shell scripts to pipe remote logs into local instance of logstalgia

These scripts make an ssh connection to a server and tail a designated Apache log file, piping the output into a local instance of Logstalgia. This gives you a visualization like this:

[![Logstalgia demo video](http://img.youtube.com/vi/HeWfkPeDQbY/0.jpg)](http://www.youtube.com/watch?v=HeWfkPeDQbY)

# Steps

* Download and install [logstalgia](http://code.google.com/p/logstalgia/)
* edit one of these scripts to point to your server log and your logstalgia instance
* enjoy

This process assumes you are set up for [public key authentication](https://hkn.eecs.berkeley.edu/~dhsu/ssh_public_key_howto.html) to get access to your account on the server.

## *nix

```
#!/bin/sh   
USER=xxxxx
SERVER=my.server.org
LOG=/var/apache2/logs/access_log
ssh $USER@$SERVER "tail -f $LOG" | logstalgia -x -1400x800 -
```

Copy that to a file named SERVER.sh, edit the USER, SERVER and LOG values, and make it executable.

## Windows with Putty Saved Session

```
@echo off
echo Running Logstalgia for xxxx logs
set PLINK="full-path-to-plink"
set USER=user
set LOGSTALGIA=path-to-logstalgia-directory
set SESSION=putty-saved-session-name
set LOG=path-to-web-server-log
%PLINK% -load %SESSION% tail -f %LOG% | %LOGSTALGIA%\logstalgia.exe --sync
```
Replace all variables with your server and log information. Save that as a .bat file, edit the variables, and run it.

##Example
```
@echo off
echo Running Logstalgia for xxxx logs
set PLINK="C:\Users\user\Documents\putty\plink.exe"
set USER=ec2-user
set LOGSTALGIA=C:\Users\user\AppData\Local\Logstalgia
set SESSION=WEBSERVER1
set LOG=/var/loh/httpd/myserver.example.org-access_log.log
%PLINK% -load %SESSION% tail -f %LOG% | %LOGSTALGIA%\logstalgia.exe --sync
```

This assumes you are running [Pageant](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html), and that you have created a PuTTY session that enables you to log into your server with a public key. See the [plink documentation](http://the.earth.li/~sgtatham/putty/0.60/htmldoc/Chapter7.html#S7.2.2) for further information.
