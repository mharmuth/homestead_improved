# Homestead Improved for PHP 7.1 and Sulu CMS

A fork of [Swaders Homestead Improved](https://github.com/Swader/homestead_improved). This version is updated to use [Sulu CMS](http://sulu.io/) and PHP 7.1.

## Prerequisites

What you need:

* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](https://www.vagrantup.com/)

Install both and start your jorney ;)

## Installation

* Clone project

        https://github.com/mharmuth/homestead_improved hi_sulu
        
* Apply folderfix to map/share folders between Host and Box

        cd hi_sulu; bin/folderfix.sh
        
* Create `Homestead.yaml` configuration file. Simply use `Homestead.yaml.dist` file as boilerplate. 
Change configuration to fit your needs.

* Add `sulu.app` (or the URL you added to your Homestead config) to your OS' host file (e.g. `/etc/hosts`). 
The corresponding IP can be taken from config file.

* Boot up the VM and SSH into it

        vagrant up; vagrant ssh
        
## Installing Sulu

**All steps here happen inside the VM!**

    cd Code
    git clone https://github.com/sulu-io/sulu-standard sulu; cd sulu
    git checkout master
    composer install
    
This will take a while, so lean back and grap a cup of coffee.

The Symfony post install scripts will ask for some parameters. The only ones we need to change right 
now are the `database_name` (homestead), the `database_user` (homestead) and the `database_password` (secret).

### Configuring Sulu

    cp app/Resources/webspaces/sulu.io.xml.dist app/Resources/webspaces/sulu.io.xml
    cp app/Resources/pages/default.xml.dist app/Resources/pages/default.xml
    cp app/Resources/pages/overview.xml.dist app/Resources/pages/overview.xml
    cp app/Resources/snippets/default.xml.dist app/Resources/snippets/default.xml
    rm -rf app/cache/*
    rm -rf app/logs/*
    
Now we have a [Sulu default webspace](http://tldrify.com/c3x).

In the file `app/Resources/webspaces/sulu.io.xml` we replace the `name` and `key` with the values we prefer.

Now we have to generate a CMS user and some other development stuff. This can be done by simply
executing

        app/console sulu:build dev
        
That's all for now. Try to open up the backend of Sulu with the following URL:

        http://sulu.app/admin/
        
This can be a bit slow as Symfony apps in a VM are always slow -.- But we'll cover that later.

For now, create the frontend assets (CSS, JS and default theme) with the following command:

        app/console assetic:dump
        
## Speed Improvements

// TODO