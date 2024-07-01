### WHY?
- Databases are key components of system design.
- Operations engineers deploy and manage databases.
- Applications read and write to these databases.

- Learn how to deploy databases.
- Understand how to connect to databases.
- Perform basic troubleshooting.

### Databases
- **MySQL**: SQL Database
- **MongoDB**: NoSQL Database

### SQL
Tabular/Relational Databases:
- When new information is added, the whole table is affected.
- Each person gets a row, and all rows in a SQL database form a table. In contrast, in NoSQL, each person gets a document, and all documents in a NoSQL database form a collection.

Example:
```sql
SELECT * FROM persons WHERE age > 10;
```

### NoSQL
Stores information in the form of documents or pages (Key-Value Pair):
- Each individual gets a document, and all information about that individual is stored in that file.

Example:
```js
db.persons.find({age: {$gt: 10}});
```

### MySQL Database
- Open source
- Fast
- Reliable
- SQL

There are two editions of MySQL: Community (free) and Commercial (for enterprises).

### Install MySQL
```bash
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
rpm -ivh mysql80-community-release-el7-3.noarch.rpm
yum install mysql-server
service mysqld start
service mysqld status
```
This is the simplest form of database installation. For production databases, additional steps are involved. Always refer to the documentation when installing for production.

### Validate MySQL Installation
```bash
cat /var/log/mysqld.log
```
It will generate a temporary password:
```bash
mysql -u root -p<temporary_password>
```
Now you are connected to the MySQL server and inside the MySQL prompt:
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```
You can also run `mysql_secure_installation` instead of altering the user.

### Basic MySQL Commands
```sql
SHOW DATABASES;
CREATE DATABASE school;
USE school;
CREATE TABLE persons (Name VARCHAR(255), Age INT, Location VARCHAR(255));
SHOW TABLES;
INSERT INTO persons VALUES ("John Doe", 45, "New York");
SELECT * FROM persons;
```

### Create User in MySQL
When first connecting to the SQL database, you use the root user, which has all permissions by default. In production environments, create new users with restricted access.
```bash
mysql -u root -p<password>
```
```sql
CREATE USER 'john'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```
This user can only connect from localhost. To allow connections from other hosts:
```sql
CREATE USER 'john'@'192.168.1.10' IDENTIFIED BY 'MyNewPass4!';
CREATE USER 'john'@'%' IDENTIFIED BY 'MyNewPass4!';
```

### Grant Privileges in MySQL
```sql
GRANT <PERMISSION> ON <DB.TABLE> TO 'john'@'%';
GRANT SELECT ON school.persons TO 'john'@'%';
GRANT SELECT, UPDATE ON school.persons TO 'john'@'%';
GRANT SELECT ON school.* TO 'john'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'john'@'%';
SHOW GRANTS FOR 'john'@'localhost';
```

### MongoDB
- Open source
- NoSQL
- Scalable
- High performance

MongoDB collects data as documents. Multiple documents form collections, and multiple collections form databases. We can have multiple databases in a single MongoDB server.

### Install MongoDB
Follow the official documentation to configure the package management system:
```bash
yum install mongodb-org
```

### Start MongoDB Service
```bash
systemctl start mongod
systemctl status mongod
```

### MongoDB Logs
```bash
cat /var/log/mongodb/mongod.log
```

### MongoDB Configuration File
```bash
/etc/mongod.conf
```

### Mongo Shell
```bash
mongo
```
By default, access control is disabled. You must perform steps to enable access control explicitly when you deploy MongoDB in production.

### Basic MongoDB Commands
```js
show dbs;
use school;
db;
db.createCollection('persons');
show collections;
db.persons.insert({"name": "John Doe", "age": 45, "location": "New York", "salary": 5000});
db.persons.find();
db.persons.find({"name": "John Doe"});
```