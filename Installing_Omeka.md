### Installing Omeka

I installed Omeka this week. The steps were easy to follow, however, I somehow made a mistake in the process. When I went to the Omeka site in my browser, it produced an error message. I could not figure out where I made a mistake, but I assumed it may have had to do with the password I created since that was one thing I was unable to double check. I tried changing the password, and that also did not fix the problem. At that point, I decided to just delete the database and directory I had created and started over. I followed the same steps and everything ended up working correctly. 

**Prerequisites**

Before installing Omeka there were a couple of prerequisites I need to install. 

* First, I had to install **ImageMagick**, which **Omeka** uses to create thumbnail images of images that are uploaded to the digial library.

```
sudo apt install imagemagick
```
* Then, I had to install `mod_write`. This is used to create user friendly URLs for items and collection in the digital library.

```
sudo a2enmod rewrite
```

* Lastly, I had to restart **Apache**

```
sudo systemctl restart apache2
```

**General Steps to Install Omeka**

* First, I created a new user and database for the **Omeka** installation. I did this by following the steps used in installing **WordPress**.

	* I switched to the **Linux root user** and logged in as the **MySQL root user**.

```
sudo su
sudo mysql -u root
```

	* Then I created the new user and database and granted all privileges to the user.

```
create user 'omeka'@'localhost' identified by 'password';
create database omeka;
grant all priviledges on omeka.* to 'omeka'@'localhost';
```

* Next, I downloaded the **Omeka** software using `wget` and unziped the zip file with `unzip`. 

```
cd /var/www/html
sudo wget https://github.com/omeka/Omeka/releases/download/v3.1.2/omeka-3.1.2.zip
sudo unzip omeka-3.1.2
```

* After this, I renamed the `omeka-3.1.2` directory to `omeka`.

```
cd /var/www/html/omeka
sudo mv omeka-3.1.2 omeka
```

* Once I changed the name, I opened the file `db.ini` in nano.

* In the `db.ini` file, I changed all of the `XXXXXX` values to corresponding information about the database and user I had created.

* Finally, I used the `chown` command to change ownership of user and owner to `www-data`.

* I then restarted **Apache2** and **MySQL**. 

* On my first atttempt, as mentioned above, I ended up getting an error message, which led to starting this whole process over. The second time, I was able to access Omeka through the web browser. 

* From there, I went to my web brower and filled out the information for creating an account. After that, I added a few books to the digital library. 

* Eventually, I went to my **WordPress** site to create a link to my **Omeka** site. I couldn't find any way to add a link. I asked in teams what others had done for that part of the assignment, but I still couldn't figure out how to add the link. Eventually, I used ChatGPT to figure out what was happening. It turns out the theme I had chosen did not allow customization. I changed themes and successfully created a link to my **Omeka** site.
