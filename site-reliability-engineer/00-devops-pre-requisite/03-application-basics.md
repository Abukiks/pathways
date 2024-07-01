# Application Basics

## Overview
This guide is designed for non-developers who want to understand and manage different types of applications. It covers building, troubleshooting, and experimenting with applications using various programming languages.

### Who is this for?
- Non-Developers

### Different Types of Applications:
- Python
- NodeJS
- Java

### Sections:
- Building Applications
- Troubleshooting
- Labs

---

## Types of Programming Languages

### Compiled Languages
Compiled languages are written in source code, which is then compiled into machine code before execution.

#### Examples:
- Java
- C
- C++

#### Java Example:

1. **Develop Source Code:**
   ```java
   public class MyClass {
     public static void main(String[] args) {
       System.out.println("Hello World");
     }
   }
   ```

2. **Compile:**
   ```sh
   $ javac MyClass.java
   # Output: MyClass.class
   ```

3. **Run:**
   ```sh
   $ java MyClass
   Hello World
   ```

### Interpreted Languages
Interpreted languages are executed directly using an interpreter, often involving implicit compilation to byte code.

#### Examples:
- Python
- NodeJS
- Ruby

#### Python Example:

1. **Develop Source Code:**
   ```python
   def print_message():
       print("Hello World")

   if __name__ == '__main__':
       print_message()
   ```

2. **Run:**
   ```sh
   $ python main.py
   Hello World
   ```

## Packages/Modules/Libraries
Applications use various packages to extend functionality. Common types include:
- Filesystem
- Math
- Operating System
- HTTP
- Security
- Networking

### Tools to Manage Dependencies:
- `npm` for NodeJS
- `pip` for Python

Understanding package management is crucial for troubleshooting dependency issues.

## Build
The build process typically involves:
1. **Compile**
2. **Run Tests**
3. **Package**
4. **Delivery**

Build procedures vary by application type:
- Python
- Java
- NodeJS

DevOps tools automate these tasks into pipelines.

---

## Java Basics

### Overview
Java is a popular programming language known for its portability, scalability, and robustness. It is free, open source, and has a large community support.

### Install Java
1. **Download and Extract JDK:**
    ```sh
    $ wget https://download.java.net/.../openjdk-13.0.2_linux-x64_bin.tar.gz
    $ tar -zvxf openjdk-13.0.2_linux-x64_bin.tar.gz
    ```

2. **Verify Installation:**
    ```sh
    $ /opt/jdk-13/bin/java -version
    ```

### Java Development Kit (JDK)
- **Develop:**
  - `jdb`: Debugger
  - `javadoc`: Documentation generator
- **Build:**
  - `javac`: Compiler
  - `jar`: Archiver
- **Run:**
  - JRE (Java Runtime Environment)
  - `java`

### Compile and Run a Java Application
1. **Develop Source Code:**
   ```java
   public class MyClass {
     public static void main(String[] args) {
       System.out.println("Hello World");
     }
   }
   ```

2. **Compile:**
   ```sh
   $ javac MyClass.java
   # Output: MyClass.class
   ```

3. **Run:**
   ```sh
   $ java MyClass
   Hello World
   ```

### Package
- **Java Archive (JAR):**
   ```sh
   $ jar cf MyApp.jar MyClass.class Service1.class Service2.class ...
   # Output: MyApp.jar
   $ java -jar MyApp.jar
   ```

- **Web Archive (WAR):** Used for web applications.

### Document
Generate documentation using `javadoc`:
   ```sh
   $ javadoc -d doc MyClass.java
   ```

## Build Process
1. **Develop**
2. **Compile** (using `javac`)
3. **Package** (using `jar`)
4. **Document** (using `javadoc`)

The build process can become complex with larger teams and more sophisticated applications.

### Build Tools
- **Maven**
- **Gradle**
- **ANT**

#### Example Build Steps:
1. **Compile**
2. **Package**
3. **Document**

Configuration files:
- `.xml` (ANT)
- `.gradle` (Gradle)

These build tools help manage the build process, dependencies, and project structure efficiently.

---

## NodeJS and Python Installation and Management Guide

## NodeJS

NodeJS is a powerful platform for building network applications. It is free, open source, and cross-platform compatible.

### Features of NodeJS
- **Free**
- **Open Source**
- **Cross Platform Compatible**

### Installing NodeJS on CentOS
To install NodeJS on CentOS, follow these steps:

1. **Setup NodeJS Repository:**
    ```sh
    $ curl -sL https://rpm.nodesource.com/setup_13.x | bash -
    ```

2. **Install NodeJS:**
    ```sh
    $ yum install nodejs
    ```

### NodeJS Commands

Verify the installation by checking the NodeJS version:
```sh
$ node -v
v13.10.1
```

### Building an Application

Example of a simple NodeJS application that adds two numbers:

```javascript
// Returns addition of two numbers
let add = function (a, b) {
    return a + b;
};

const a = 10, b = 5;
console.log("Addition: " + add(a, b));
```

Run the application:
```sh
$ node add.js
Addition: 15
```

### Node Package Manager (NPM)
NPM is the package manager for NodeJS, used to install and manage packages.

#### NPM Features
- Files
- Web Servers
- Databases
- Security
- Many More

### NPM Commands

Install a package:
```sh
$ npm install file
```

Use the package in your application:
```javascript
var file = require("file");
file.mkdirs("/tmp/dir1");
```

Check module paths:
```sh
$ node -e "console.log(module.paths)"
['/app/node_modules', '/node_modules']
```

Install a package globally:
```sh
$ npm install file -g
```

### Common Modules 

#### Built-In Modules
List built-in modules:
```sh
$ ls /usr/lib/node_modules/npm/node_modules/
```
Common built-in modules:
- `fs`: To handle filesystem operations
- `http`: To host an HTTP server
- `os`: To work with the operating system
- `events`: To handle events
- `tls`: To implement TLS and SSL
- `url`: To parse URL strings

#### External Modules
List external modules:
```sh
$ ls /usr/lib/node_modules/
```
Common external modules:
- `express`: Fast, unopinionated, minimalist web framework
- `react`: To create user interfaces
- `debug`: To debug applications
- `async`: To work with asynchronous JS
- `lodash`: To work with arrays, objects, strings, etc.

### Application Dependencies

Define application dependencies in `package.json`. Ensure to install supported dependencies.

---

## Python

Python is a versatile programming language known for its readability and ease of use. It is free, open source, and cross-platform compatible.

### Features of Python
- **Free**
- **Open Source**
- **Cross Platform Compatible**

### Installing Python on CentOS

#### Versions
- Python2 (2000-2010)
- Python3 (2008 - Present)

Install Python2:
```sh
$ yum install python2
```
Use Python2:
```sh
$ python2
```
Check Python2 version:
```sh
$ python2 -V
```

Install Python3:
```sh
$ yum install python36
```
Use Python3:
```sh
$ python3
```
Check Python3 version:
```sh
$ python3 -V
```

### Python Commands

Example of a simple Python script:
```python
def print_message():
    print("Hello World")

if __name__ == '__main__':
    print_message()
```

Run the script using Python2:
```sh
$ python2 main.py
Hello World
```

### Python Package Manager (pip)

Check Python versions:
```sh
$ python2 -V; python3 -V
```

Check pip versions:
```sh
$ pip2 -V; pip3 -V
```

Install a package using pip:
```sh
$ pip install flask
```

Show package details:
```sh
$ pip show flask
```

Check Python module paths:
```sh
$ python2 -c "import sys; print(sys.path)"
```

### Requirements File
Specify dependencies in a `requirements.txt` file:
```
Flask==0.10.1
Jinja2==2.7.3
MarkupSafe==0.23
Werkzeug==0.9.6
requests==2.3.0
gunicorn==18.0
```

Install all dependencies listed in `requirements.txt`:
```sh
$ pip install -r requirements.txt
```

### Upgrade/Uninstall Package

Upgrade a package:
```sh
$ pip install flask --upgrade
```

Uninstall a package:
```sh
$ pip uninstall flask
```

### Other Package Managers

- **easy_install**
  - Use `easy_install`

 to manage Python packages:
    ```sh
    $ easy_install app.egg
    ```
- **wheels**
  - Use `wheels` to manage Python packages:
    ```sh
    $ pip install app.whl
    ```

By following these guidelines, you can efficiently manage your NodeJS and Python environments on CentOS, ensuring smooth development and deployment processes.