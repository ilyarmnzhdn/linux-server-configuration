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
