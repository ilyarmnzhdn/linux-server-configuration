# Linux Server Configuration
Final project for Udacity Full Stack Web Development Nanodegreee
This is my final project for Udacity Full Stack Web Development Nanodegree project. In this project, a Linux virtual machine needs to be configurated to support the Item Catalog website (Which I made as third project for Udacity).

You can visit http://52.29.83.1 for the website deployed
Accessible SSH port: 2200

## Tasks

1. Launch your Virtual Machine with your Udacity account
2. Follow the instructions provided to SSH into your server
3. Create a new user named grader
4. Give the grader the permission to sudo
5. Update all currently installed packages
6. Change the SSH port from 22 to 2200
7. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
8. Configure the local timezone to UTC
9. Install and configure Apache to serve a Python mod_wsgi application
10. Install and configure PostgreSQL:
	- Do not allow remote connections
	- Create a new user named catalog that has limited permissions to your catalog application database
11. Install git, clone and setup your Catalog App project (from your GitHub repository from earlier in the Nanodegree program) so that it functions correctly when visiting your server’s IP address in a browser. Remember to set this up appropriately so that your .git directory is not publicly accessible via a browser!

## Launch Virtual Machine

## Instructions for SSH access to the instance

Download Private Key

Move the private key file into the folder ~/.ssh (where ~ is your environment's home directory). So if you downloaded the file to the Downloads folder, just execute the following command in your terminal. mv ~/Downloads/udacity_key.rsa ~/.ssh/

Open your terminal and type in chmod 600 ~/.ssh/udacity_key.rsa

In your terminal, type in ssh -i ~/.ssh/udacity_key.rsa root@52.29.83.1

Development Environment Information

Public IP Address

52.29.83.1

## Create a new user named grader

1. sudo adduser grader
2. vim /etc/sudoers
3. touch /etc/sudoers.d/grader
4. vim /etc/sudoers.d/grader, type in grader ALL=(ALL:ALL) ALL, save and quit

## Set ssh login using keys

1. generate keys on local machine usingssh-keygen ; then save the private key in ~/.ssh on local machine

2. deploy public key on developement enviroment

On you virtual machine:
```
$ su - grader
$ mkdir .ssh
$ touch .ssh/authorized_keys
$ vim .ssh/authorized_keys
```

Copy the public key generated on your local machine to this file and save
```
$ chmod 700 .ssh
$ chmod 644 .ssh/authorized_keys
```
3. reload SSH using `service ssh restart`
4. now you can use ssh to login with the new user you created

`ssh -i [privateKeyFilename] grader@52.29.83.1`

## Update all currently installed packages

	sudo apt-get update
	sudo apt-get upgrade

## Change the SSH port from 22 to 2200
1. Use `sudo vim /etc/ssh/sshd_config` and then change Port 22 to Port 2200 , save & quit.
2. Reload SSH using `sudo service ssh restart`

## Configure the Uncomplicated Firewall (UFW)

Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)

	sudo ufw allow 2200/tcp
	sudo ufw allow 80/tcp
	sudo ufw allow 123/udp
	sudo ufw enable 
	
## Configure the local timezone to UTC
1. Configure the time zone `sudo dpkg-reconfigure tzdata`
2. It is already set to UTC.

## Install and configure Apache to serve a Python mod_wsgi application
1. Install Apache `sudo apt-get install apache2`
2. Install mod_wsgi `sudo apt-get install python-setuptools libapache2-mod-wsgi`
3. Restart Apache `sudo service apache2 restart`

## Install and configure PostgreSQL
1. Install PostgreSQL `sudo apt-get install postgresql`
2. Check if no remote connections are allowed `sudo vim /etc/postgresql/9.3/main/pg_hba.conf`
3. Login as user "postgres" `sudo su - postgres`
4. Get into postgreSQL shell `psql`
5. Create a new database named catalog  and create a new user named catalog in postgreSQL shell
	
	```
	postgres=# CREATE DATABASE catalog;
	postgres=# CREATE USER catalog;
	```
5. Set a password for user catalog
	
	```
	postgres=# ALTER ROLE catalog WITH PASSWORD 'password';
	```
6. Give user "catalog" permission to "catalog" application database
	
	```
	postgres=# GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;
	```
7. Quit postgreSQL `postgres=# \q`
8. Exit from user "postgres" 
	
	```
	exit
	```

## Install git, clone and setup your Catalog App project.
1. Install Git using `sudo apt-get install git`
2. Use `cd /var/www` to move to the /var/www directory 
3. Create the application directory `sudo mkdir itemCatalog`
4. Move inside this directory using `cd itemCatalog`
5. Clone the Item-Catalog App to the virtual machine `git clone https://github.com/ilyarmnzhdn/item-catalog.git itemCatalog`
6. Move to the inner itemCatalog directory using `cd itemCatalog`
7. Rename `application.py` to `__init__.py` using `sudo mv application.py __init__.py`
8. Edit `database_setup.py` and change `engine = create_engine('sqlite:///itemcatalog.db')` to `engine = create_engine('postgresql://itemcatalog:password@localhost/itemcatalog')`
9. Install pip `sudo apt-get install python-pip`
10. Use pip to install dependencies `sudo pip install -r requirements.txt`
11. Install psycopg2 `sudo apt-get -qqy install postgresql python-psycopg2`
12. Create database schema `sudo python database_setup.py`

## Configure and Enable a New Virtual Host
1. Create itemCatalog.conf to edit: `sudo nano /etc/apache2/sites-available/itemCatalog.conf`
2. Add the following lines of code to the file to configure the virtual host. 
	
	```
	<VirtualHost *:80>
		ServerName 52.29.83.1
		ServerAdmin 1030516030@erciyes.edu.tr
		WSGIScriptAlias / /var/www/itemCatalog/itemCatalog.wsgi
		<Directory /var/www/itemCatalog/itemCatalog/>
			Order allow,deny
			Allow from all
		</Directory>
		Alias /static /var/www/itemCatalog/itemCatalog/static
		<Directory /var/www/itemCatalog/itemCatalog/static/>
			Order allow,deny
			Allow from all
		</Directory>
		ErrorLog ${APACHE_LOG_DIR}/error.log
		LogLevel warn
		CustomLog ${APACHE_LOG_DIR}/access.log combined
	</VirtualHost>
	```
3. Enable the virtual host with the following command: `sudo a2ensite itemCatalog`

## Create the .wsgi File
1. Create the .wsgi File under /var/www/itemCatalog: 
	
	```
	cd /var/www/itemCatalog
	sudo nano itemCatalog.wsgi 
	```
2. Add the following lines of code to the flaskapp.wsgi file:
	
	```
	#!/usr/bin/python
	import sys
	import logging
	logging.basicConfig(stream=sys.stderr)
	sys.path.insert(0,"/var/www/itemCatalog/")

	from itemCatalog import app as application
	application.secret_key = '12345'
	```

## Restart Apache
1. Restart Apache `sudo service apache2 restart `
