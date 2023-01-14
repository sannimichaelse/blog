# How to Install MongoDB on  CentOS 8

## Introduction 

MongoDB is a cross-platform document-oriented database program. Classified as a NoSQL database program, MongoDB is great for transactional stores where performance is a concern. It's also great when the data structure is going to evolve over time, as its schema-less operations allow you to update the data on the fly.

This tutorial guides you through installing and configuring MongoDB Community Edition on a CentOS 8 server.

## Prerequisites

Before following this tutorial, make sure you have a regular, non-root user with sudo privileges. You can learn more about how to set up a user with these privileges from our guide, How to Create a Sudo User on CentOs


## Install MongoDB Community Edition

Follow these steps to install MongoDB Community Edition using the yum package manager.

## Step 1 - Configure the package management system (yum)

MongoDB is not available in CentOS 8 core repositories. We’ll enable the official MongoDB repository and install the packages.

At the time of writing this article, the latest version of MongoDB available from the official MongoDB repositories is version 4.2. Before starting with the installation, visit the Install on Redhat section of MongoDB’s documentation and check if there is a new release available

Create an /etc/yum.repos.d/mongodb-org-4.2.repo file so that you can install MongoDB directly using yum:


```
[mongodb-org-4.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.2.asc
``` 

Save and close the file. You can confirm if the it was added successfully by running

``` 
yum repolist
``` 

Output

``` 
repo id                          repo name
base/7/x86_64                    CentOS-7 - Base
extras/7/x86_64                  CentOS-7 - Extras
mongodb-org-4.2/7                MongoDB Repository
updates/7/x86_64                 CentOS-7 - Updates
``` 

## Step 2 – Installing MongoDB
#### Install the mongodb-org meta-package:

To install the latest stable version of MongoDB, issue the following command:

``` 
sudo yum install mongodb-org
``` 

There are two Is this ok [y/N]: prompts. The first one permits the installation of the MongoDB packages and the second one imports a GPG key. At each point, Click on Y and Enter

The following packages will be installed on your system as a part of the mongodb-org package:

- **mongodb-org-server **- The mongodb daemon, and corresponding init scripts and configurations.
- **mongodb-org-mongos** - The mongos daemon.
- ** mongodb-org-shell**  - The mongo shell, an interactive JavaScript interface to MongoDB, used to perform administrative tasks through the command line.
- **mongodb-org-tools** - Contains several MongoDB tools for importing and exporting data, statistics, as well as other utilities.

## Step 3 – Start and Enable the Mongo Service
Next, start the MongoDB service with the systemctl utility:

``` 
sudo systemctl start mongod
``` 

You can also use systemctl to check that the service has started properly.

``` 
sudo systemctl status mongod
``` 

You should see something like actively running

``` 
Active: active (running) since Mon 2020-04-27 18:13:17 EDT; 2h 31min ago
``` 

#### Enable MongoDB

This will automatically start MongoDB when the system reboot.

``` 
sudo systemctl enable mongod
``` 

## Step 4 – Verify Installation
To verify the installation, connect to the MongoDB database server and print the server version:

``` 
mongo
``` 

Run the following command to display the MongoDB version:

``` 
db.version()
``` 

The output will look like something like this:

``` 
4.2.6
``` 

## Step 5 – Configure MongoDB

The MongoDB configuration file is named mongod.conf and is located in the /etc directory. The file is in YAML format.

The default configuration settings are sufficient in most cases. However, for production environments, its recommend to uncomment the security section and enable authorization as shown below:

``` 
security:
       authorization:enabled
``` 

The authorization option enables Role-Based Access Control (RBAC) that regulates users access to database resources and operations. If this option is disabled, each user will have access to any database and execute any action.

After making changes to the MongoDB configuration file, restart the mongod service:

``` 
sudo systemctl restart mongod
``` 

## Step 6 – Creating Administrative MongoDB User

If you enable MongoDB authentication, you’ll need to create an administrative user that can access and manage the MongoDB instance.
First, access the MongoDB shell with:

``` 
mongod
``` 

Type the following command to connect to the admin database:

``` 
use admin
``` 

switched to db admin

Create a new user named mongoAdmin with the userAdminAnyDatabase role:
``` 
db.createUser(
 {
   user: "mongoAdmin",
   pwd: "yourpassword",
   roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
 }
)
``` 

OUTPUT

``` 
Successfully added user: {
	"user" : "mongoAdmin",
	"roles" : [
		{
			"role" : "userAdminAnyDatabase",
			"db" : "admin"
		}
	]
}
``` 

You can name the administrative MongoDB user as you want.

Exit the mongo shell with:

``` 
quit()
``` 

To test the changes, access the mongo shell using the administrative user you have previously created:

``` 
mongo -u mongoAdmin -p --authenticationDatabase admin
``` 

-u flag is for users
-p flag for password
--authenticationDatabase for database to be authenticated

Enter password

Next, Type the command  below

``` 
use admin
``` 

switched to db admin

Now, print the users with:

``` 
show users
``` 

``` 
{
	"_id" : "admin.mongoAdmin",
	"user" : "mongoAdmin",
	"db" : "admin",
	"roles" : [
		{
			"role" : "userAdminAnyDatabase",
			"db" : "admin"
		}
	],
	"mechanisms" : [
		"SCRAM-SHA-1",
		"SCRAM-SHA-256"
	]
}
``` 

Conclusion
In this guide you ve learnt how to install and configure MongoDB 4.2 on your CentOS 8 server. You can Consult The MongoDB 4.2 Manual for more information on this topic.















