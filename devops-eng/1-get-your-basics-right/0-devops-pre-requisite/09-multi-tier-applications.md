## Two-Tier Application Deployment using LAMP Stack

This guide provides a step-by-step process for deploying an eCommerce application using the LAMP stack (Linux, Apache, MariaDB, PHP). The deployment will follow the order of Linux firewall setup, MariaDB installation and configuration, Apache setup, and PHP configuration.

### Step 1: Install Firewall

First, we will install and configure the firewall on the Linux server.

#### Install Firewall
```bash
sudo yum install firewalld
```

#### Start and Enable the Firewall
```bash
sudo service firewalld start
sudo systemctl enable firewalld
sudo systemctl status firewalld
```

### Step 2: Install and Configure MariaDB

Next, we will install and configure MariaDB.

#### Install MariaDB
```bash
sudo yum install mariadb-server
```

#### Configure MariaDB
Edit the MariaDB configuration file to set the appropriate port.
```bash
sudo vi /etc/my.cnf
```

#### Start and Enable MariaDB
```bash
sudo service mariadb start
sudo systemctl enable mariadb
sudo systemctl status mariadb
```

#### Configure Firewall for MariaDB
```bash
sudo firewall-cmd --permanent --zone=public --add-port=3306/tcp
sudo firewall-cmd --reload
```

#### Secure MariaDB Installation
It is important to secure your MariaDB installation to remove insecure default settings and configure access permissions properly.
```bash
sudo mysql_secure_installation
```

#### Configure the Database
```sql
mysql
MariaDB > CREATE DATABASE ecomdb;
MariaDB > SHOW DATABASES;
MariaDB > CREATE USER 'ecomuser'@'localhost' IDENTIFIED BY 'ecompassword';
MariaDB > GRANT ALL PRIVILEGES ON ecomdb.* TO 'ecomuser'@'localhost';
MariaDB > FLUSH PRIVILEGES;
```

#### Load Data
```bash
mysql -u ecomuser -p ecomdb < db-load-script.sql
```

### Step 3: Install and Configure Apache HTTP Server

After setting up MariaDB, we will install and configure the Apache HTTP Server and PHP.

#### Install Apache and PHP
```bash
sudo yum install -y httpd php php-mysql
```

#### Configure Firewall for HTTPD
```bash
sudo firewall-cmd --permanent --zone=public --add-port=80/tcp
sudo firewall-cmd --reload
```

#### Configure HTTPD
Edit the HTTPD configuration file to use `index.php` as the DirectoryIndex.
```bash
sudo vi /etc/httpd/conf/httpd.conf 
# Configure DirectoryIndex to use index.php instead of index.html
```

#### Start and Enable HTTPD
```bash
sudo service httpd start
sudo systemctl enable httpd
sudo systemctl status httpd
```

#### Download Application Code
```bash
sudo yum install -y git
git clone https://github.com/<application>.git /var/www/html/
# Update index.php to use the correct database address, name, and credentials
```

#### Test the Setup
```bash
curl http://localhost
```

### Step 4: Install and Configure PHP

Finally, we configure the PHP application.

#### Update PHP Configuration
Edit the PHP configuration file to set any necessary parameters.
```bash
sudo vi /etc/php.ini
```

#### Test PHP
Create a test PHP file to ensure PHP is correctly installed and configured.
```bash
echo "<?php phpinfo(); ?>" > /var/www/html/info.php
curl http://localhost/info.php
```
Remove the `info.php` file after testing for security reasons.
```bash
rm /var/www/html/info.php
```

### Multi-Node Deployment Model

In a multi-node setup, the web server (Apache, PHP) is deployed on one node (e.g., `172.20.1.102`), and the database (MariaDB) is deployed on another node (e.g., `172.20.1.101`). The instructions remain largely the same, with adjustments for configurations and connectivity.

#### Web Server Configuration
Update the `index.php` file on the web server to connect to the remote database server.
```php
<?php
    $link = mysqli_connect('172.20.1.101', 'ecomuser', 'ecompassword');

    if ($link) {
        $res = mysqli_query($link, "SELECT * FROM products;");
        while ($row = mysqli_fetch_assoc($res)) {
            // Process rows
        }
    }
?>
```

#### Database Server Configuration
Allow the web server to connect to the MariaDB server by configuring the IP address.
```sql
mysql
MariaDB > CREATE DATABASE ecomdb;
MariaDB > CREATE USER 'ecomuser'@'172.20.1.102' IDENTIFIED BY 'ecompassword';
MariaDB > GRANT ALL PRIVILEGES ON ecomdb.* TO 'ecomuser'@'172.20.1.102';
MariaDB > FLUSH PRIVILEGES;
```

### Summary

The primary modification in the multi-node deployment is updating the `index.php` connection string to use the IP address of the MariaDB server.

### References
- [GitHub - KodeKloudHub/Learning-App-Ecommerce](https://github.com/kodekloudhub/learning-app-ecommerce)