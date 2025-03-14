# **Mini Portfolio Module 4**

## **What is a LAMP stack?**

**LAMP** is an acronym that stands for Linux Apache MySQL PHP.
**Linux** is the operating system.
**Apache** is the web server.
**MySQL** is a database management system.
**PHP** is the server-sided programming language.

It is commonly used for creating web servers that have extra funcitonality through PHP and MySQL.

**Installing and configuring Apache**

The command to install Apache is `sudo apt install apache2`.
After installing it, I checked to ensure it was up and running: `systemctl status apache2`.
If it is working, the output will indicated that it is Active and Loaded.
Once Apache was running, I created a web page. 
First, I installed two command line web browsers, `w3m` and `elinks`. There were installed using: `sudo apt install w3m elinks` 
Then, I checked to see if Apache had been installed and working correctly by looking at the VM's private IP address: `w3m 10.128.0.99`
The Apache2 Ubuntu Default Page will show if Apache is working correctly.

After this, I went to the graphical browser by going to the external IP address on my VM. If this also showed the Apache2 Ubuntu Default Page, then that is another indication Apache is working as it should. 

I then created a webpage. I changed the name of the index.html page and changed the information in the original file using nano. 

I added html code with everything we wanted to diplay on the webpage. 
Then, I went to the external IP address. If everything was working correctly, then the webpage would contain the information entered into the index.html file. 
This was the only place that I had trouble. When I went to my IP address, the correct information was not showing. 
Other than the typical typos, I did not have issues with installing or configuring Apache.

**Installing and configuring PHP**

To install PHP along with libapache2-mod-php packages: `sudo apt install php libapache-mod-php`
To create a connection between PHP and Apache: `sudo systemctl restart apache2`
To confirm the installed version: `php -v`
To check for errors in log output: `systemctl status apache2`

Next, check to see that PHP is installed and working with Apache.
First, create a PHP file in document root: `cd /var/www/html/` `sudo nano info.php`
To the file, add:
 
`<?php 
phpinfo();
?>`

Save, close and open VM's external IP address. If the php info document show, that means that PHP has been installed and is working with Apache.
I then deleted info.php: `sudo rm /var/www/html/infor.php`

After then, I changed the Apache default page from index.html to index.php
To do this, I had to edit the `dir.conf` file in the `/ect/apache2/mods-enabled/` directory.
I saved the orginal file and then used nano change information in the `dir.conf` file:
```cd /etc/apache2/mods-enabled/
sudo cd dir.conf dir.conf.bak
sudo nano dir.conf```
 
I had to change the order of index.php and index.html so that the php file came first: 
`DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm`
I then checked the configuration using `apachectl`: 
`apachectl configtest`

I received the OK message which meant I could then reload the Apache configuration, restart the service and check the status:
```sudo systemctl reload apache2
sudo systemctl restart apache2
systemclt status apache2```

After this, I created an index.php file using nano: 
```cd /var/www/html/
sudo nano index.php```

I entered a combination of html and php code into nano, saved it, and went to the external IP to see if everything worked. 
This is where I had an issue. The html information was still showing, not php. Eventually it turned out that I needed to restart Apache, then return to the IP address. This quickly fixed the issue.

**Installing and configuring MySQL**

To install MySQL Community Server package: `sudo apt install mysql-server`
Check for which versions are available if you need a specific one: `apt policy mysql-server`
Confirm the version you are using: `mysql --version`
Check to see if MySQL is successfully running and enabled: `systemctl status mysql`
Run a post installation script to perform securty check and make sure everything is secure: `sudo mysql_secure_installation`

After this I was able to log into the database: `sudo mysql -u root`
Then I created an `opacuser` regular user account: 
`create user 'opacuser'@'localhost' 'identified by 'XXXXXXXX';`
After this, I created a database and granted `opacuser`all privileges:
```create database opacdb default character set utf8mb4 collate utf8mb4_unicode_ci;
show databases;
grant all privileges on opacdb.* to 'opacuser'@'localhost' with grant option;```

Next, I had to exit the bash shell and entire the MySQL server:
`nano .bashrc`
To the bottom of the file, I added:
`export MYSQL_PS1="[\d]> "`
Then, save the source file and exited:
`source ~/.bashrc`
After this, I was able to log back into MySQL with the `opacuser` account. From there, I was able to create a table and practice editing it. 
I did not run into any issues with this part of the assignment. It all went pretty smoothly.
