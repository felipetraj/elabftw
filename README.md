![elabftw logo](http://i.imgur.com/hq6SAZf.png)

###[Official website](http://www.elabftw.net) | [Live demo](https://demo.elabftw.net)

# Description

**eLabFTW** is an electronic lab notebook manager for research teams. It also features a database where you can store any kind of objects (think antibodies, plasmids, cell lines, boxes, _etc_…)
It is accessed _via_ the browser by the users. Several research teams can be hosted on the same install.

Tired of that shared excel file for your antibodies or plasmids ?
Want to be able to search in your past experiments as easily as you'd do it on google ?
Want an electronic lab notebook that lets you timestamp legally your experiments ?
Then you are at the right place !

**eLabFTW** is designed to be installed on a server, and people from the team would just log into it from their browser.
Don't have a server ? That's okay, you can use an old computer with 1 Go of RAM and an old CPU, it's more than enough. Just install a recent GNU/Linux distribution on it and plug it to the intranet.

Don't have an old computer ? That's okay, you can install eLabFTW on a Raspberry Pi (you can buy one on [Radiospares](http://www.rs-components.com/index.html)). It's a 30€ computer on which you can install GNU/Linux and run a server in no time ! That's what we use in our lab. Check out the [wiki](https://github.com/NicolasCARPi/elabftw/wiki/raspberrypi) to know more.


Keep in mind that eLabFTW is currently in beta and is under heavy developpement. Your input is very welcome :)
Please report bugs on [github](https://github.com/NicolasCARPi/elabftw/issues).

Thank you for choosing eLabFTW as a lab manager =)

# Installation
## The legendary four steps installation instructions (for advanced users)
* [Download the latest stable version](https://github.com/NicolasCARPi/elabftw/releases/latest/)
* Extract it on your web server
* Create a MySQL database and a MySQL user for elabftw
* Go to https://your-address.org/elabftw/install

## Install on your computer (Mac/Win)
* [Install locally on Mac](https://github.com/NicolasCARPi/elabftw/wiki/installmac).
* [Install locally on Windows](https://github.com/NicolasCARPi/elabftw/wiki/installwin).

## Install on a digitalocean's drop (easiest/quickest method)
With this method, you can have a running elabftw server in no time. You need to purchase a `drop` from [DigitalOcean.com](https://www.digitalocean.com/pricing/). It starts at 5$/month. This setup is enough to run eLabFTW for a team or more.
Everything is explained here : 
* [Install eLabFTW on a drop](https://github.com/NicolasCARPi/drop-elabftw#how-to-use)

## Install in a docker container
If you know Docker already and want to use a dockerized elabftw, please see [this repo](https://github.com/NicolasCARPi/elabftw-docker-nosql#elabftw-docker-nosql).

## Install on a GNU/Linux server
Please refer to your distribution's documentation to install :
* a webserver (like Apache, nginx, lighttpd or cherokee)
* php version > 5 with the following extensions : gettext, gd, openssl, hash
* mysql version > 5.5
* git

The quick way to do that on a Debian/Ubuntu setup :
~~~ sh 
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install mysql-server-5.6 mysql-client apache2 php5 php5-mysql libapache2-mod-php5 phpmyadmin git
~~~

Make sure to put a root password on your mysql installation :
~~~ sh
$ sudo /usr/bin/mysql_secure_installation
~~~


### Getting the files

The first part is to get the files on your server, with git.

#### Connect to your server with SSH
~~~ sh
ssh user@12.34.56.78
~~~

#### Cd to the public directory where you want eLabFTW to be installed
(can be /var/www, ~/public\_html, or any folder you'd like, as long as the webserver is configured properly, in doubt use /var/www)
~~~ sh
$ cd /var/www
# make the directory writable by your user (if it's not already the case)
$ sudo chown `whoami`:`whoami` .
~~~
Note the `.` at the end that means `current folder`.

#### Get latest stable version via git :
~~~ sh
$ git clone --depth 1 https://github.com/NicolasCARPi/elabftw.git
~~~
(this will create a folder `elabftw`)
The `--depth 1` option is to avoid downloading the whole history.

If you cannot connect, try exporting your proxy settings in your shell like so :
~~~ sh
$ export https_proxy="proxy.example.com:3128"
~~~
If you still cannot connect, tell git your proxy :
~~~ sh
$ git config --global http.proxy http://proxy.example.com:8080
~~~

Alternatively, you can [download a stable version](https://github.com/NicolasCARPi/elabftw/releases/latest).

But git will allow for easier updates (and they are frequent !).

### SQL part
The second part is putting the database in place.
#### Command line way (graphical way below)
~~~ sh
# first we connect to mysql
$ mysql -u root -p
# we create the database (note the ; at the end !)
mysql> create database elabftw;
# we create the user that will connect to the database.
mysql> grant usage on *.* to elabftw@localhost identified by 'YOUR_PASSWORD';
# we give all rights to this user on this database
mysql> grant all privileges on elabftw.* to elabftw@localhost;
mysql> exit
~~~
You will be asked for the password you put after `identified by` three lines above.

*<- Ignore this (it's to fix a markdown syntax highlighting problem)


#### Graphical way with phpmyadmin
You need to install the package `phpmyadmin` if it's not already done.

**Note**: it is not recommended to have phpmyadmin installed on a production server (for security reasons).

~~~sh
$ sudo apt-get install phpmyadmin
~~~

Now you will connect to the phpmyadmin panel from your browser on your computer. Type the IP address of the server followed by /phpmyadmin.

Example : https://12.34.56.78/phpmyadmin

Login with the root user on PhpMyAdmin panel (use the password you setup for mysql root user).
##### Create a user `elabftw` with all rights on the database `elabftw`

Now click the `Users` tab and click ![add user](http://i.imgur.com/SJmdg0Z.png).

Do like this :

![phpmyadmin add user](http://i.imgur.com/kE1gtT1.png)


### Final step
Finally, point your browser to the install folder (install/) and read onscreen instructions.

For example : http://12.34.56.78/elabftw/install

******

# Post install things to do
You can read [this page](https://github.com/NicolasCARPi/elabftw/wiki/finalizing) to finish fully the configuration of your install.

# Updating
To update, just cd in the `elabftw` folder and do :
~~~ sh
$ git pull
$ php update.php
~~~

![bad time](http://i.imgur.com/aUzNvIg.jpg)

# Backup
It is important to backup your files to somewhere else, in case anything bad happens.
Please refer to the [wiki](https://github.com/NicolasCARPi/elabftw/wiki/backup).

# Bonus stage
* It's a good idea to use a php optimizer to increase speed. I recommand installing XCache.
* You can show a TODOlist by pressing 't'.
* You can duplicate an experiment in one click.
* You can export in a .zip, a .pdf or a spreadsheet.
* You can share an experiment by just sending the URL of the page to someone else.
* Experiments can be locked by your PI


~Thank you for using eLabFTW :)
Please open a github issue if you have any problem (or send me an email !).

http://www.elabftw.net

\o/
