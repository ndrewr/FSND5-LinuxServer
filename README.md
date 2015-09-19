Udacity Fullstack Nanodegree - Project 5
=====================================================
### Linux Server Configuration - Ubuntu x Apache AWS server instance ###
### Author: Andrew Roy Chen, Sept 2015 ###



SUMMARY:
-----------------------------------------------------
Configured a base Ubuntu installation to serve FSND3's Catalog Flask application on an AWS virtual machine instance. HTTP server used is Apache 2 with the mod_wsgi module backed by a Postgresql database. 
The Catalog app can be visited at:
[http://ec2-52-11-99-253.us-west-2.compute.amazonaws.com/](http://ec2-52-11-99-253.us-west-2.compute.amazonaws.com/)

Note: the app uses GitHub authentication to authorize post creation and editing.



ACCESS:
-----------------------------------------------------
The assigned IP is 52.11.99.253
Server can be acccessed via SSH through port 2200 with the user name 'grader'. 

```
ssh -p 2200 grader@52.11.99.253
```

Auth key information is included separately.



INSTALLED SOFTWARE:
-----------------------------------------------------
This project began as a fresh installation of Ubuntu 14.04 but a number of software additions were needed to complete objectives including:

- ntp
- apache2, libapache2-mod-wsgi, python-setuptools
- git
- pip
- virtualenv
- python-dev, libpq-dev
- postgresql, postgresql-contrib

Within the Python virtual environment the Flask app had additional requirements listed in:
~/var/www/yama/requirements.txt

These app-specific installations were done with the pip command:
```
pip install -r requirements.txt
```



CONFIGURATION STEPS:
-----------------------------------------------------
The configuration process was done in roughly this sequence:

**CREATED AWS instance, downloaded Udacity RSA key**

- (from Local machine) login to root@52.11.99.253 using udacity RSA key
- (on server) adduser *grader*
- add file to sudoers.d with user permission ALL
- switched to *grader* account
- (back on Local dev machine) ssh-keygen to create RSA key pair
- (back on server) created .ssh directory in /home
- created authenticated_keys file
- set ownership of dir and file
- manual copy of public rss key info

**CAN NOW LOGIN AS grader**

- add backdoor with my IP => for contingency
- In *sshd_config*: changed port to 2200
- config UFW for port 2200, 80, 123
- enabled UFW
- UTC time set with dpkg-reconfigure tzdata
- installed ntp for auto time-sync
- updated all software with apt-get update/upgrade
- installed apache2, libapache2-mod-wsgi
- verified apache2 working
- installed git, virtualenv, python-dev, libpq-dev
- git cloned FSND3 to ~/var/www
- removed .git
- create yama.wsgi file in ~/var/www/yama/
- installed pip, virtualenv (via pip)

**APP directory structure set**

- installed postgresql, postgresql-contrib
- created user *yama* to access app postgresql db
- switched to user *postgres* (created by default)
- enter postgresql
- (in postgresql) created DB ‘yama' with pw ‘noborimasho’ and DB ROLE ‘yama’
- set DB privileges for ROLE ‘yama’
- (exit postgresql) updated Flask app settings with db location
- run default_course_data.py to bootstrap the app db

**DB setup and initialized**

- activated app’s virtualenv and installed from req.txt with pip, deactivated
- create and enable virtual host config file yama.conf in /etc/apache2/sites-available/
- reload apache 2
- tried to visit page at site ip => server failure
- used ‘tail /var/log/apache2/error.log’ cmd to debug => venv modules missing
- [flask docs explained needed config] (http://flask.pocoo.org/docs/0.10/deploying/mod_wsgi)
- modified yama.wsgi with path to venv files

**Catalog app now loads at site ip**

- retrieved Host name from IP address
- modified yama.conf with Server alias
- updated Github user auth with Host name info
- disabled root remote login in ssh/sshd_config
- deleted backdoor with my IP



RESOURCES:
----------------------------------------------------
I needed copious resources including the Udacity Linux Server config course, FSND Project 5 forums and Fullstack Slack channel counsel and Google. 

Additionally the following specific resources:

-https://discussions.udacity.com/t/markedly-underwhelming-and-potentially-wrong-resource-list-for-p5/8587

-https://discussions.udacity.com/t/p5-how-i-got-through-it/15342

-https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-12-04

-https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server

-https://help.ubuntu.com/community/UFW

-https://help.ubuntu.com/community/UbuntuTime

-https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps

-http://blog.trackets.com/2013/08/19/postgresql-basics-by-example.html

-https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps

-http://flask.pocoo.org/docs/0.10/deploying/mod_wsgi

