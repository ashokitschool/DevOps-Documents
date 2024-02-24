# Setup MySql DB Server on AWS EC2 Instance (Ubuntu) #

## Step-0: Create AWS EC2 VM using Ubuntu AMI & Connect to it using MobaXterm

## Step-1: Update the system ##
```
sudo apt update
```
## Step-2: Install MySql ##
```
sudo apt install mysql-server
```
## Step-3: Check the Status of MySql (Active or Inactive) ##
```
sudo systemctl status mysql
```
## Step-4: Login to MySql as a root ##
```
sudo mysql
```
## Step-5: Update the password for the MySql Server ##
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'place-your-password-here';
```
## Step-6: Test the MySql server if it is working by running sample sql queries
```
CREATE DATABASE ashokit;

USE ashokit;

CREATE TABLE emp (id INT, name VARCHAR(45));

INSERT INTO emp VALUES(1, 'Virat'), (2, 'Sachin'), (3, 'Dhoni'), (4, 'ABD');

SELECT * FROM emp;
```
