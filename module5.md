### Module 5 Mini Portfolio

**Introduction:**

An OPAC is a database that users can access online to look up bibliographic records related to items contained within a library catalog. Its role is similar to the traditional card catalog system. Instead of the information begin stored on physical cards that someone has to be in the library to access, the information is stored in tables in a database that can be accessed online. An OPAC allows someone to search the catalog without having to be physically present at the library. 

**Relational Databases:**

Relational databases store bibliographic records in tables. These databases are linked together through primary and foreign keys. Each category of bibliographic information is separated into its own column where individual values are placed. For example, there is likely a title column that contains all of the titles for each book in separate fields. In a relational database, there are multiple tables that are connected to each other through primary and foreign keys. This allows the connected tables to all be searched when using MySQL to query specific information. These databases interact with the OPAC through a PHP file that activates MySQL to perform a query to find the information entered into the OPAC search bar. 

**Step-by-Step Setup:**

-First, create a search page using HTML code. I created a file called “mylibrary.html” where the html code was placed. This code allows the construction of a webpage that indicates the information the user will need to search the catalog. For example, a search bar and date fields to search copyright dates. 
	
-Then, a file entitled “search.php” is created. The line  `<form method="post" action="search.php">` links the html page to the php.search page. The php code entered into nano is used to enable MySQL to query the database for the information placed by the user in the webpage.
 
-When someone submits the information on the search page, the PHP script will be activated. This will activate MySQL which will query the database to find and retrieve the search information. 

-Once the information has been retrieved, the information contained in the table will be displayed to the user on the webpage.

-Additional steps to make the OPAC more real-world like would be connecting to other databases. We created a very small, simple database, but in a real OPAC there would be multiple databases that could be search. The records would also be structured using MARC. More fields would also be able to be searched, which would allow for more advanced searching capabilities. 

-To make the cataloging module more real-world like, there would be more tables in the database to store more data. The graphical interface would also be more complex. CSS and JavaScript would be used to make the interface more user friendly and stylized. 

**Key Details:**

Here are some of the key details that ensure the system operates functionally: 

-the php file must be referenced in the html file in order for them to work together. In the OPAC mylibrary.html file, the code, <a href="opac.php">OPAC</a>, creates a link between the html file and the php file. Without this link, the php file, that enables the database to be searched, could not be accessed.

-the same goes for the cataloging module. The code, `<form action="insert.php" method="post">`, entered into the html file creates a connection to the php file, which allows the database to be searched.

-one important detail in the cataloging module is that the form created through the html file must mimic the data structure of the books table. For example, the fields we created were author, title, publisher, and copyright. This means those are the fields that must be contained in the form. 

-another important detail in making the catalog password protected, is to find this block of code in the apache2.config file 

```
<Directory /var/www/>
  Options Indexes FollowSymLinks
  AllowOverride None
  Require all granted
</Directory>
```

And change “None” to all. I somehow messed up when doing this and I was not asked for a username and password when I access my external IP address. Once I discovered this problem, I was able to fix it and it worked. 
