# Infrastructure Guide

This guide covers the infrastructure build for hosting on AWS. 

## AWS EC2

Create an EC2 instance using Amazon Linux AMI. For proof of concept the nano size will work but may be too slow. t3.micro is recommended for general use.

## AWS Network 

As part of the instance configuration allow traffic from all for port 80. The public IPV4 address is available on the dashoard. 

_Note if the instance type is changed, the IP address will change._

## SSH client and access keys

Downloaded key must have permissions modified to chmod 600

Connect with:

`ssh -i "BabyLab.pem" ec2-user@[public_dns_from_dashbard]`

## Apache Server

This should come up whe the instance is rebooted.

`sudo service httpd restart`

## MySQL Database Server

Add MySQL to the start up

`sudo chkconfig mysqld on`

Check what services are set for each run state

`sudo chkconfig --list`

Manual start up

`sudo service mysqld start`

## FTP client

Start Filezilla and select File >> Site Manager...

* Click the New Site button
* Name the site
* On the General tab:
    * For Host enter the server name or IP address (either will work)
    * Leave the port blank unless you have been given a specific port.
    * Select SFTP under Servertype
    * Select Normal for Logintype
    * Enter the User name and Password
    * Click OK
* None of the other tables need changes

The first time you login you will be prompted to accept a “key”. Click Ok and the key will be stored and you will not be prompted again.

## DNS

Name registration and DNS are at domain.com
Login ***********
Password: **********

## Database password

Configured in .env in the aplication root folder /cctapp

DB_DATABASE=cctapp

DB_USERNAME=***********

DB_PASSWORD=**************

## PHPmyAdmin

Upload the PHPmyAdmin files to the /phpmyadmin subdirectory
http://[IP or Domain Name]/phpmyadmin/

Set the database credentials to 

DB_USERNAME=**********

DB_PASSWORD=**************

## General Online Documentation

https://cctapp.readthedocs.io/en/latest/





