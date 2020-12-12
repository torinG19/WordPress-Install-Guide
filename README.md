#  How to Install WordPress on Ubuntu 16.04
## Prerequisites
* ubuntu 16.04 server
* ssh root access with sudo privileges
## Step 1: Get Updates
Update:

Run the following command
```
sudo apt-get update
```
## Step 2: Install Apache2

install Apache2:

Run the following command
```
sudo apt-get install apache2 
```
## Step 3: Create Domain Name in Your Hosts File
Run this command and add your IP address and domain 
```
sudo nano /etc/hosts
```
Example: 189.27.270  mywebsite.com

save by hitting "control o" then hit "enter" exit by hitting "control x"

## Step 4: Copy Default Apache2 Configuration File For Your New Domain Name Configuration

Run this command with your own domain
```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/yourdomain.com.conf
```
## Step 5: Add Entries To Your Configuration File
Run this command with your domain name and un-comment server name by deleting the # in front and add your server name
```
sudo nano /etc/apache2/sites-available/yourdomain.com.conf 
```
save by hitting "control o" then hit "enter" exit by hitting "control x"
## Step 6: Disable The Default Configuration And Enable Our New Configuration
Run these commands in a row replacing with your domain name
```
sudo a2dissite 000-default.conf
```
```
sudo a2ensite yourdomain.com.conf
```
```
sudo systemctl reload apache2
```
## Step 7: Test By Writing a Simple PHP Script in Root Folder.
You will have to delete the Apache page (your default index.HTML)
```
sudo rm index.html
```
Create a index.php file
```
sudo nano index.php
```
add some text. Somthing like "hello world"

save by hitting "control o" then hit "enter" exit by hitting "control x"
## Step 8: Install PHP
Run this command and then restart Apache2 by running the second command
```
sudo apt-get install php7.0 libApache2-mod-php7.0 php7.0-mysql php7.0-curl php7.0-mbstring php7.0-gd php7.0-xml php7.0-xmlrpc php7.0-intl php7.0-soap php7.0-zip
```
```
sudo systemctl restart apache2
```
## step 9: install MySQL
```
sudo apt-get install mysql-server
```
enter "Y" to continue

set password for root user
## Step 10: Set Up Secure Login
Run this command
```
sudo mysql_secure_installation
```

* Enter root password
* You will be asked if you would like to enable “VALIDATE PASSWORD PLUGIN”. Enter “Y” for yes, “N” for no.

If you enable, options for security parameters will be presented for user passwords. You will be given these options: 0 (minimum of 8 characters), 1(minimum of 8 characters and must include capital letters, special characters and numbers), 2 (minimum of 8 characters and must include capital letters, special characters, numbers and dictionary checks). Read more about MySQL’s password validation policy: https://dev.MySQL.com/doc/refman/5.7/en/validate-password-options-variables.html#sysvar_validate_password_policy.



* Change the password for root? : “N” (we already set this).
* Remove anonymous users? : “Y”
* Disallow root login remotely? : Select “Y: if you do not want to disallow and “N” if you want to allow  root account remote access
* Remove test database and access to it? : “Y”
* Reload privilege tables now? : “Y”

Start and enable mySQL
```
sudo systemctl start mysql
```
```
sudo systemctl enable mysql
```
## Step 11: Create a WordPress Database
Run this command to log in as root
```
sudo mysql -u root -p
```
Create a new database with your chosen name and replace databasename with it
```
CREATE DATABASE databasename;
```
Create a new user
```
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
```
Grant new user privileges
```
GRANT ALL PRIVILEGES ON databasename.* TO 'wordpressuser'@'localhost';
```
Check to see if it was created. You should see your new database name
```
SHOW DATABASES;
```
exit MySQL
```
FLUSH PRIVILEGES;
```
```
EXIT;
```
Restart Apache2
```
sudo systemctl restart apache2
```
Restart MySQL
```
sudo systemctl restart mysql
```
## Step 12: Install WordPress
Go into your root directory

Download WordPress
```
sudo wget -c http://wordpress.org/latest.tar.gz
```
Extract the file
```
sudo tar -xzvf latest.tar.gz
```
Correct wordpress permissions
```
sudo chown -R www-data:www-data /var/www/html/wordpress/
```
```
sudo chmod -R 755 /var/www/html/wordpress/
```
## Step 13: Configure WordPress
Go into the WordPress folder 
```
cd /var/www/html/wordpress
```
Rename the sample configuration file:
```
sudo mv wp-config-sample.php wp-config.php
```
Open the configuration file
```
sudo nano /var/www/html/wordpress/wp-config.php
```
Enter your MySQL details in the following portion of the file:
```
// * MySQL settings - You can get this info from your web host * //
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'wordpressuser');

/** MySQL database password */
define('DB_PASSWORD', 'user_password_here');

/** MySQL hostname */
define('DB_HOST', 'localhost');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');
```
save by hitting "control o" then hit "enter" exit by hitting "control x"

Move WordPress files to root directory
```
cd /var/www/html
```
```
sudo rsync -av wordpress/* /var/www/html/
```
## Step 14: Complete WordPress Installation in The Browser
Go to your domain and follow the steps to setting up WordPress
