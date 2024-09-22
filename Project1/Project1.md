
## __LAMP PROJECT__

LAMP stack is a collection of Linux OS, Apache Server, MySQL Database, PHP. LAMP is basically a collection of software that you require to create a dynamic website and web applications. These tools are capable enough that you don’t require any other tool for the purpose. The interesting thing about LAMP is that all the tools in it are free and open-source.

### STEP 1: INSTALL APACHE 

- Update the repositries and install Apache using Ubuntu's package manager by running command: 
 
```
sudo apt update 
sudo apt install apache2
```
                                                                                                  

- Verify that apache2 is running as a Service in the OS: 

```
sudo systemctl status apache2
```

*Apache2 status below* 
![Project 1- C (2)](https://user-images.githubusercontent.com/93116204/138972237-0c769ceb-4afe-44ce-abfe-28d2e87b8560.png)

- Requested Apache HTTP server on port 80: 

```
curl http://localhost:80
```

- Verify if Apache HTTP server can respond to requests from the Internet. 

*Screenshot below*
![defaultpage](https://user-images.githubusercontent.com/93116204/138972873-9a287524-61b1-4506-9e6e-58232be745c3.png)



### STEP 2: INSTALL MYSQL AND SETUP ROOT PASSWORD

- MYSQL is a popular relational database management system used within PHP enviroments
                                         
                                         
- Install MySQL by running this command: 

```
sudo apt install mysql-server
```                                       
                                         
                                                             
- Run a security script that comes pre-installed with MySQL: 

```
sudo mysql secure installation 
```                                                           
                                                                     
- Log into MySQL to test connection: 

```
sudo mysql
```   

- I then exited MySQL

```
mysql> exit
```                     
                     
 *Screenshot below*
                     
 ![Project1 - D (6)](https://user-images.githubusercontent.com/93116204/139803926-a89676ef-24d3-4fd5-8c1b-bdf258614988.png)
                    
 
### STEP 3: INSTALL PHP

- PHP is the component of our setup that will process code to display dynamic content to the end user.

- In addition to the php package, the server will need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases.

- libapache2-mod-php will be required to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

- These 3 packages were installed at once by running: 

```
sudo apt install php libapache2-mod-php php-mysql
```
                                                                                                   
 - confirmed my PHP version by running:

 ```
 php -v
 ```
                                                                           
   *Screenshot below* 
   
  ![Project1 - G (3)](https://user-images.githubusercontent.com/93116204/139135928-1216975b-7405-43d8-be96-29b392ab3404.png)

- To test the setup with a PHP script, set up a Apache Virtual Host to hold the website’s files and folders. 

- The objective is to setup a domain called ‘projectlamp’.

- Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory. 

- Add a directory next next to the default one.

``` 
sudo mkdir /var/www/projectlamp
```
                                                              
 - Assign ownership of the directory    

```
sudo chown -R $USER:$USER /var/www/projectlamp   
```
                                                                  
 - Create virtual host configuration file in Apache’s sites-available directory
 
 ``` 
 sudo vi /etc/apache2/sites-available/projectlamp.conf
 ```
 
Insert the following bare-bones configuration

```
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Saved and closed the file, as shown below:

1. Hit the esc button on the keyboard

1. Type :

1. Type wq (w means write and q means quit)

1. Hit ENTER to save the file

```
sudo ls /etc/apache2/sites-available
```

*Screenshot below shows the new file in the site-available directory. With this VirtualHost configuration, we’re telling Apache to serve projectlamp using /var/www/projectlampl as its web root directory.* 
![Project 1 - F (2)](https://user-images.githubusercontent.com/93116204/139141359-ff767a0c-969e-4e27-8f7b-59df825621f2.png)


- enable the new virtual host
 
 ```
 sudo a2ensite projectlamp
 ```                                                           
 
 *Screenshot below*                                                          
![Project1 - G (4)](https://user-images.githubusercontent.com/93116204/139143110-b8438846-7b52-45e0-a8f4-25716ad20dc7.png)

- Disable the default website that comes installed with Apache. If this is not done Apache’s default configuration would overwrite the virtual host. 

 ```
 sudo a2dissite 000-default
 ```                                           

- To make sure the configuration file doesn’t contain syntax errors 

  ```                                                                                 
  sudo apache2ctl configtest
  ```                                                                                 
                                                                                   
- Reload Apache so these changes take effect 

```
sudo systemctl reload apache2
```     
                                                 

*Screenshot below*                                               
![Project1 - G (5)](https://user-images.githubusercontent.com/93116204/139145212-6c4570d6-138f-4021-a878-a56e0fc59a17.png)
                                              

- Create an index.html file in that location so that you can test that the virtual host works as expected:

```
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```


### STEP 4: ENABLE PHP ON THE WEBSITE

- With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. To change this behavior, edit the /etc/apache2/mods-enabled/dir.conf file and changed the order in which the index.php file was listed within the DirectoryIndex directive.

```
sudo vim /etc/apache2/mods-enabled/dir.conf
```

```
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

- Reload Apache so the changes take effect:

``` 
sudo systemctl reload apache2
```
                                             
- Create a PHP test script to confirm that Apache is able to handle and process requests for PHP files by opening a new file named index.php inside the custom web root folder:
 
``` 
vim /var/www/projectlamp/index.php
```       

- Enter the code below inside the file

```
<?php
phpinfo();
```

- Refresh ypu EC2 public ip address url page
# done done done 
*Screenshot below*
![Project 1 (2)](https://user-images.githubusercontent.com/93116204/139148367-d847b8c7-f1be-43a1-b38b-d8cbc03ab1f7.png)

