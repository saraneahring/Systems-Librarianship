### Module 5 Mini Portfolio

**Introduction:**

An OPAC is a database that users can access online to look up bibliographic records related to items contained within a library catalog. Its role is similar to the traditional card catalog system. Instead of the information begin stored on physical cards that someone has to be in the library to access, the information is stored in tables in a database that can be accessed online. An OPAC allows someone to search the catalog without having to be physically present at the library. 

**Relational Databases:**

Relational databases store bibliographic records in tables. Each category of bibliographic information is separated into its own column where individual values are placed. For example, there is likely a title column that contains all of the titles for each book in separate fields. In a relational database, there are multiple tables that are connected to each other through primary and foreign keys. This allows the connected tables to all be searched when using MySQL to query specific information. The information is broken up into separate tables to makes it easier to update and delete information.  These databases interact with the OPAC through a PHP file that activates MySQL to perform a query to find the information entered into the OPAC search bar. 

**Step-by-Step Setup:**

* First, create a search page using HTML code. I created a file called **mylibrary.html** where the HTML code was placed. This code allows the construction of a webpage that indicates the fields that can be used to search the catalog. For example, there is a search bar and date fields to search copyright dates. 
	
* Then, a file entitled **search.php** is created. The line  `<form method="post" action="search.php">` links the HTML page to the **php.search** page. The PHP code entered into nano is used to enable MySQL to query the database for the information placed by the user in the webpage.
 
* When someone submits the information on the search page, the PHP script will be activate MySQL which will query the database to find and retrieve the search information. 

* Once the information has been retrieved, the information contained in the table will be displayed to the user on the webpage.

* Additional steps to make the OPAC more real-world like would be connecting to other databases. We created a very small, simple database, but in a real OPAC there would be multiple databases that could be search. The records would also be structured using MARC. More fields would also be able to be searched, which would allow for more advanced searching capabilities. 

* To make the cataloging module more real-world like, there would be more tables in the database to store more data. The graphical interface would also be more complex. CSS and JavaScript would be used to make the interface more user friendly and stylized. 

* Some necessary configuration steps when setting up the database:

	* First, the  user must be granted privileges to be able to create a database, which requires logging in as the root user.

	```
	sudo mysql -u root

	```
   
	* Then, through the root account, a database can be created and privileges can be granted on the new database to the regular user.

	```
	mysql> create database DatebaseName;
	mysql> grant all privileges on DatabaseName.* to 'regularusername'@'localhost';

 	```

	* Now, the root MySQL account can be exited, and the regular account can be logged in to. The database is not ready to be used.

* Some necessary configuration steps when setting up the OPAC and catalog modules:

	* Creating a username and password in order to log in to the cataloging module is one example: 
			
	* First, an authentication file in the **/etc/apache2** directory is created. This where configuration files are stored for Apache2. We created the username libcat using the following code:

	```
	sudo htpasswd -c /etc/apache2/.htpasswd libcat

	```

	* Then, an authentication file in the **/etc/apache2** directory is created. This file includes the username and a hashed password. Using the libcat username, this is the code that we used:
	

	```
	sudo htpasswd -c /etc/apache2/.htpasswd libcat

	```
	
	* After this, we have to inform Apache2 that the `htpasswd` is what we will use to control access to the catalogging module. This is done in a text editor.
	
	```
	sudo nano /etc/apache2/apache2.conf

	```
		
	* Next, in the apache2.conf file, go to	line 172 where the following text is found:

	```
	<Directory /var/www/>
	 Options Indexes FollowSymLinks
 	 AllowOverride None
 	 Require all granted
	</Directory>
	
	```
	
	* **None** must be changed to **All** 
	
	* Then, go to the cataloging directory and create a file called .htaccess with the text editor
	
	```
	cd /var/www/html/cataloging
	sudo nano .htaccess
	
	```
	
	* Add the following text:
	
	```
	AuthType Basic
	AuthName "Authorization Required"
	AuthUserFile /etc/apache2/.htpasswd
	Require valid-user
	
	```
	
	* Next, if the configuration file is ok, restart Apache2
			
	* Anohter configuration step involves permission and ownership.
	
	* First the ownership of **/var/www/html/** can be changed to **www-data**, which is Apache2's user account.
	
	```
	sudo chown :www-data /var/www/html
	
	```
	
	* Then make it so that when new files or directories are created in **/var/www/html**, they will inherit the group ownership of **www-data**. 
	
	```
	sudo chmod -R g+s /var/www/html
	
	```
		
**Key Details:**

Here are some of the key details that ensure the system operates functionally: 

* One important detail in the cataloging module is that the form created through the HTML file must mimic the data structure of the books table. For example, the fields we created were author, title, publisher, and copyright. This means those are the fields that must be contained in the form. 

* For both the OPAC and cataloging modules, an important step is to make sure that the HTML file links to the PHP file.
 
	* the PHP file must be referenced in the html FILE in order for them to work together. In the OPAC **mylibrary.html** file, the code, <a href="opac.php">OPAC</a>, creates a link between the HTML file and the PHP file. Without this link, the PHP file, that activates MySQL and enables the database to be searched, could not be accessed.

	* the same goes for the cataloging module. The code, `<form action="insert.php" method="post">`, entered into the **index.html** file creates a connection to the PHP file, which allows the database to be searched.


**Using Documentation:**

Although some the PHP code used in the OPAC and cataloging modules wasn't too difficult to decipher, there were some things I needed to look up to try and understand better.  

* I kept seeing the word `echo` and looked up what it meant. I found that it is just telling the machine to print what follows `echo` to the screen. 
For example, the following code means to print the table with the results, but if there are no results, print "No results found".  

```
echo "</table>";
        } else {
            echo "<p>No results found.</p>";
        }
```

* I wanted to know what `Prepared statement to prevent SQL injection` meant. I found that SQL injection is a way that hackers can access a database and potentially steal data or destroy the database.
The code that follows this gives information for how SQL injection will be prevented. 
This is the code that follows

```
$stmt = $conn->prepare("SELECT * FROM books 
                                WHERE (author LIKE ? OR title LIKE ? OR publisher LIKE ?) 
                                AND copyright BETWEEN ? AND ?");

        // Use wildcard search
        $search_param = "%$search%";
        $stmt->bind_param("sssss", $search_param, $search_param, $search_param, $start_date, $end_date);
        $stmt->execute();
        $result = $stmt->get_result();
```
From what I understand, this means that the exact search terms the user inputs will not be used in the query. Instead `?` will be used as a placeholder. The next part of the code establishes the search parameters. `%` will be added around the search term. The next parameter takes the `?` and binds it to an actual value, which are listed in the parenthesis. This is so that the information the user input is not treated as code, but data. This enables the prevention of SQL injection. The last couple of lines are pretty straightforward. They just all the query to be executed and return results.  

Honestly, ChatGPT ended up being the best way I found to look up what the code meant. I was told in an another class that it can be inaccurate for looking up code, but it seemed very thorough in providing explanations. It was easier than trying to do a Google search where there might be an answer, but not the same thorough explanations. I couldn't really find a website other than ChatGPT that would allow me to input the code and return explanations. I suppose this may be an example when ChatGPT can be useful considering I was just using it to learn more about the code rather than using it to search for code to use, and I did not just cope and paste the answers. After I received an answer, I would use Google to try and confirm the meaning. This seemed to be a good way to double check that the information I was finding was correct.

The "Security" section under the cataloging module something else that I had a bit more difficult grasping. 

* I did not know what htpasswd meant, but the the `man htpasswd` was helpful in understanding this more. htpasswd is used to create and update files where passwords are stored. It also allows the passwords to be encrypted. 

* I also did not understand what **.htaccess** meant. I just searched Google to try and find out why this was file that was created. I found that it means "Hyper Access" and that it is a configuration file. It different uses, but we used it to allow our sites to be password protected. 

Other than the information listed above that wasn't explained in the lectures or text, I didn't really come across gaps in the provided materials. I thought it was all pretty straigthforward and easy to understand. 
 
