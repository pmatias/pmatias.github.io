---
layout: post
title:  "Installing Drush on Dreamhost shared server"
date:   2014-05-27 21:15:38
categories: drush drupal tips
share: true
---

So a year ago I've done a freelance drupal job, you know, the kind of quick-and-dirty-money-freelance-job that you completely wipe of your memory after is done, and it came back to bite my ass, no surprises there. So I needed to change some stuff over here and fix other over there, but a year ago I didn't even bother on using drush, now I can't live without it on my Drupal projects.  


A  quick word about Drupal on Dreamhost: *don't*, really, _don't do it_. The Dreamhost shared servers are *so* overselled that are completely drained of resources. So if you don't want to have a painfull experience with a CMS like Drupal, Wordpress, Joomla, whatever-thing-that-uses-lots-of-tables, just don't hire the shared services. Go for the VPS o Cloud services. Or better yet, go for [Digital Ocean](http://digitalocean.com).  


Anyway, quick steps for installing Drush on your Dreamhost shared account. First of all, you will need to login via SSH to your host, also, since is a shared account, you need to make this changes only for you. Also, you will be installing drush via PEAR, so we will set up PEAR first.  

- Create an instance of PEAR

`pear config-create ${HOME} ${HOME}/.pearrc`   
`pear install -o PEAR`   

- Edit your ~/.bash_profile file and add the following lines at the end of it  
`export PHP_PEAR_PHP_BIN=/usr/local/php53/bin/php`   
`export PATH=${HOME}/pear:/usr/local/php53/bin:${PATH}`  

- Reload your bash profile   
`. ~/.bash_profile`   

- Now install that cheeky Drush  
`pear channel-discover pear.drush.org`   
`pear install drush/drush`   

**All done. Piece of cake.**   
I took all this easy instructions from [Robin Monks](http://robinmonks.com/2012/02/installing-drush-on-a-shared-dreamhost-account/). So all credits for him.   
Also, he suggests all the instructions in one single line, pretty neat, isn't it?   

    pear config-create ${HOME} ${HOME}/.pearrc;pear install -o PEAR;echo "export PHP_PEAR_PHP_BIN=/usr/local/php53/bin/php" >> ~/.bash_profile;echo 'export PATH=${HOME}/pear:/usr/local/php53/bin:${PATH}' >> ~/.bash_profile;. ~/.bash_profile;peuar channel-discover pear.drush.org;pear install drush/drush
