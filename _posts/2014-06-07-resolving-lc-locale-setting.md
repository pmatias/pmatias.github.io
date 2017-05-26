---
title:  "Solving the 'Setting locale failed' error"
date:   2014-05-27 21:15:38
categories: digital-ocean ubuntu linux
share: true
comments: true
---

Every time that I start a new droplet in [DO](http://www.digitalocean.com/)(very happy with the service so far, thank you very much), I have this annoying message every time I try to make a system change(installing packages, for instance):

``` bash     
perl: warning: Setting locale failed.    
perl: warning: Please check that your locale settings:    
  LANGUAGE = (unset),    
  LC_ALL = (unset),    
  LC_TIME = "pt_BR.UTF-8",   
  LC_MONETARY = "pt_BR.UTF-8",   
  LC_CTYPE = "en_US.UTF-8",   
  LC_ADDRESS = "pt_BR.UTF-8",   
  LC_TELEPHONE = "pt_BR.UTF-8",   
  LC_NAME = "pt_BR.UTF-8",   
  LC_MEASUREMENT = "pt_BR.UTF-8",   
  LC_IDENTIFICATION = "pt_BR.UTF-8",   
  LC_NUMERIC = "pt_BR.UTF-8",   
  LC_PAPER = "pt_BR.UTF-8",   
  LANG = "en_US.UTF-8"   
    are supported and installed on your system.   
perl: warning: Falling back to the standard locale ("C").   
locale: Cannot set LC_ALL to default locale: No such file or directory   
```     
*(the message could be slightly different depending on your environment)*   
This not new, I remember seeing this message back in my old days of Slackware, and the simplest solution is to edit the `/etc/environment` file and add the following line
```   
LC_ALL="en_GB.UTF-8"    
```   
(you should check which locale files are installed_-with the `locale -a` command-_ on your droplet before setting that variable)    
Now, as far as I know, logging out and in(or just calling `source /etc/environment`) should do the trick, but rebooting a droplet in DO takes like literally 15 seconds, and at this point it is mostly certain that you droplet it's in its early configuration stages so rebooting will cause no harm :)
