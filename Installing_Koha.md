### Install Koha Assignment

To install **Koha**, which is an open source integrated library system, I had to create a new **VM Instance**. This is because **Koha** requires more memory and disk space than the **VM Instance** I have already been using.

I started by following the instructions I used to set up the first **VM Instance**, and then made a few changes mentioned below. 
* For **Series**, I used **E2**, set the **Machine Type** to **2CPU, 4GB memory**, and incresed the disk size to **20GB**. 
* Then, under **Networking**, I chose **Allow HTTP traffic** and added `koha-8080` to the **Network tags** box. 
	

Next, I created a **firewall rule** to allow access to the staff interface indentified by port 8080. 
* To do this, I went to **VPN Network** and chose **Firewall**. 
* Next, I clicked on **Create a firewall rule** and added `koha-opac` and the description **Open port 8080 for the OPAC**.
* Then, by **Targets**, I clicked on **Specificed target tags** and added `koha-8080` in **Target tags**
* In **Source IPv4 ranges**, I added 0.0.0.0/0
* Finally, under **Specified protocols and ports**, I chose **TCP**, added **8080** to the **Ports** box, and hit **Create**

After this, I needed to prepare the server for installing **Koha**. 
* As always, I ran the update and upgrade commands first.
* Next, I needed to run the `apt autoremove` command and `apt clean` command to clear up disk space. 

```
sudo apt autoremove -y && sudo apt clean
```

* Then, I needed to install `gnupg2` and `apt-transport-https`. `gnupg2` helps to create more secure communication. 

```
sudo apt install gnupg2 apt-transport-https
```

Then, I added the **Koha repository** to sync with the local repository database and use to download software. 
* First, I switched to the `root` user. 

```
sudo su
```

* Next, I added the Koha repsitory.

```
echo 'deb http://debian.koha-community.org/koha stable main' | sudo tee /etc/apt/sources.list.d/koha.list
```

* After that, I downloaded and installed the GPG key, which is used for encrypting and decrypting data, in the `trusted.gpg.d` directory.

```
wget -qO- https://debian.koha-community.org/koha/gpg.asc | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/koha.gpg > /dev/null
```
 
* Lastly, I checked to make sure the GPG key contained an email from `koha-community.org`.

```
gpg --show-keys /etc/apt/trusted.gpg.d/koha.gpg
```

Next, I installed **Koha**.
* I used `apt update` to ensure the new repository synced with the `Koha` repository.
* Next I viewed the package information for **Koha** and installed it. 

```
apt show koha-common
apt install koha-common
```

 Then, I configured **Koha**
* First, I created a backup to the default configuration file.

```
cd /etc/koha/
cp koha-sites.conf koha-sites.conf.backup
```

* Then, I opened the configuration file with **nano**

```
nano /etc/koha/koha-sites.conf
```

* Next, in the file, I changed `Intraport="80"` to `Intraport="8080"` 
*  After that, I installed and set up `mysql-server`

```
apt install mysql-server
```

* Then, I set the root **MySQL** password by replacing the `XXXXXXXX` with my chosen password.

```
mysqladmin -u root password MyPassword
```

* Next, I had to enable URl rewriting and CGI funtionality and restarted **Apache2**.

```
a2enmod rewrite
a2enmod cgi 
systemctl restart apache2
```

* After that, I created a database for **Koha** called `bibliolib`.

```
koha-create --create-db bibliolib
```

* Then, I told **Apache2** to listen on port 8080 by opening the `ports.conf` file on **nano** and changing `Listen 80` to `Listen 8080`.

```
nano /etc/apache2/ports.conf 
```

* I checked to make sure the changed were valid and restarted **Apache2**.

```
apachectl configtest
systemctl restart apache2
```

* After that I needed to disable the default **Apache2** setup. I used the `deflate` command to enable traffic compresssion, enabled the **bibliolib** site, reloaded **Apache2's** configureations and restarted. 

```
a2dissite 000-default
a2enmod deflate
a2ensite bibliolib
systemctl reload apache2
systemctl restart apache2
```

* The last step on the back end was to find the **Koha** username and password in the `koha-conf.xml` file so that I could login to the **Koha** web interface. I opened the file and found the `<config>` stanza on line 252. On lines 257 and 258, I found the username and password.

```
nano /etc/koha/sites/bibliolib/koha-conf.xml
```
	
* Finally, I was able to go to the web server to finish the installation. 

I followed the steps and created an **Adminstrator identity** at the end of the setup. 

This is where I hit an issue. I followed the steps below to change the settings in order to view my public facing OPAC. However, when I went to my external IP Address, it said that it could not connect to the server. 

* I selected **More** from the top drop down box. 
* Then, I chose **Administration**.
* Next, I clicked on **System Preferences**
* I found **OPAC** on the side bar.
* I scrolled down to `OPACBaseURL`, entered my IP Address, http://35.222.164.82 and hit **Save all OPAC Preferences**  
	
After going through the documentation a couple of times, I could not figure out where I went wrong. Eventually, I posted on Teams asking if anyone could help me figure out where I went wrong. After some time trying to pinpoint the issue, Dr. Burns was able to find that I port 80 was not open. This was because when adding Listen 8080 to the port.conf file, I deleted Listen 80. I was supposed to keep Listen 80 so that port 80 would remain open in addition to port 8080. After this fix, I was able to access my public facing OPAC.
