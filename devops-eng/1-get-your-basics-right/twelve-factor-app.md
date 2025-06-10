## Why 12 Factor App?

You built an application to get your idea out to the world. Traditionally, you had to go through hoops and hurdles to get your idea out to the world. It would take you days, weeks, or even months to order and receive a server to host your application on. And eventually, once you get a server to host your application on, you were dependent on that server for the rest of your life. Your application and your server were married for life.

You wrote code that could live on that server. It couldn't live on other servers. And if you had to scale, you would scale resources on that server. Heck, you even stored session information on that server. If the user was in the middle of something and your server crashed, everything that the user did until that point was lost and they had to do all of that all over again. Your app was tied to that server for life.

Fast forward many decades to today. We are living in the world of high-growth SaaS startups that see user growth from zero to millions in a matter of a few months. Going from an idea to execution only takes as much time as you need to develop the code for it. Provisioning and hosting can be achieved in a matter of hours, if not minutes. Most cloud platforms today can provision servers and other resources in a matter of minutes. And most of the time, you don't even need to worry about provisioning a server. With PaaS and serverless technologies, you can simply write code and push — you're done.

Today's platforms are expected to be up 99.999% of the time, meaning there is no time to take down the application for any reason whatsoever. No time to take down the application for patching servers, adding additional resources, or scaling. For this to happen, your application needs to break free from the underlying infrastructure. This way you can host your application anywhere you want — be it on-prem, GCP, AWS, or Azure.

And that's known as portability — the ability to run the same app on different environments without having to change the source code of the application.

When it comes to scaling, in the past, you had to take down your application to add additional resources to your servers. That was referred to as vertical scaling. Today, you provision more servers and run more instances of your application — horizontal scaling.

To summarize, modern applications need to be portable and must not be tightly coupled with the underlying infrastructure. They should have minimal divergence when deployed between dev, test, and prod environments to enable continuous deployment. They must be easily scalable by spinning up many instances at once and must be suitable for deployment on modern cloud platforms.

In order to achieve this, your application needs to be developed keeping certain principles in mind. About a decade ago, engineers from Heroku put together 12 factors that need to be considered when building modern applications. These are known as the 12-Factor App principles.

And that's what we will see with examples in the rest of the documentation. These are also documented at the 12factor.net website.

---
# Introduction to the Twelve-Factor App

In this documentation, we will explore the Twelve-Factor App methodology through a simple and easy-to-understand example using Python and the Flask web framework.

The Twelve-Factor App is a methodology for building software-as-a-service apps that:

* Use declarative formats for setup automation
* Have a clean contract with the underlying operating system
* Offer maximum portability between execution environments
* Are suitable for deployment on modern cloud platforms
* Minimize divergence between development and production
* Can scale up without significant changes to tooling, architecture, or development practices

## The 12 Factors

Here is a list of the twelve factors:

1. **Codebase** - One codebase tracked in revision control, many deploys.
2. **Dependencies** - Explicitly declare and isolate dependencies.
3. **Config** - Store config in the environment.
4. **Backing Services** - Treat backing services as attached resources.
5. **Build, Release, Run** - Strictly separate build and run stages.
6. **Processes** - Execute the app as one or more stateless processes.
7. **Port Binding** - Export services via port binding.
8. **Concurrency** - Scale out via the process model.
9. **Disposability** - Maximize robustness with fast startup and graceful shutdown.
10. **Dev/Prod Parity** - Keep development, staging, and production as similar as possible.
11. **Logs** - Treat logs as event streams.
12. **Admin Processes** - Run admin/management tasks as one-off processes.

---

In the following sections, we will go through each of these principles with basic examples using Flask to demonstrate the ideas in a concrete and approachable way.


## The 12-Factor App: Single Codebase

### I. Codebase

```app.py
from flask import Flask

app = Flask(__name__)

@app.route('/')  
def welcomeToAbukiks():
    return "Welcome to Abukiks!"

if __name__ == "__main__": 
    app.run(host="0.0.0.0", debug=True)
```

Our simple Flask app, currently residing on your laptop in the `app.py` file, is the entry point for our web application. It handles incoming and outgoing requests and displays "Welcome to Abukiks!" in a browser.

### Collaboration Challenges

As user demand grows and more developers join, collaboration becomes challenging. Developers working in isolation and copying code to a central hub leads to conflicts and inefficiencies.

### Git to the Rescue

Git helps developers collaborate efficiently on the same application. Each developer installs and configures Git, pulls the latest code from the central hub, adds their changes, and pushes them back.

### Central Hubs

Cloud-based platforms like GitHub serve as central repositories for code. GitHub allows configuration of projects, organizations, users, and access levels. GitLab and BitBucket are similar platforms.

### Codebase per Application

In the past, a single codebase often housed multiple applications and services. However, the 12-Factor App methodology dictates that each application should have its own codebase. Multiple applications sharing the same code violate this principle and should be separated.

### Multiple Deployments

Within each codebase, multiple deployments can exist for different environments such as development, staging, and production. The same codebase is used across these deployments.

---

## The 12-Factor App: Explicitly Declare and Isolate Dependencies

The second rule of the 12-Factor App methodology emphasizes that applications should **explicitly declare and isolate all dependencies**. This means your app should never rely on the implicit existence of system-wide packages. Instead, all necessary components must be clearly defined and managed to ensure consistency across various environments.

### II. Dependencies

For Python applications, you typically list dependencies in a file named `requirements.txt`. This file specifies the package names and their exact version numbers.

Here's a simple example:

```requirements.txt
flask==2.0.0
```

**Specifying the version number** is critical. Without it, different developers or deployment environments might install varying versions of a package, leading to inconsistencies and unexpected behavior. Imagine a scenario where a developer uses one version of Flask during development, but by the time the app is deployed to production, a newer version has been released. If the operations team installs the latest version without explicit versioning, your app might not function as expected.

During the build process, the command `pip install -r requirements.txt` will install all the dependencies listed in this file. The `requirements.txt` file is usually located in the **root directory** of your project.

### Isolating Dependencies

Declaring dependencies is only half the battle; isolating them is equally important. Consider developing two separate applications on your laptop where each requires a different version of the same dependency:

```App1
Flask==2.0.0
```

```App2
Flask==1.9.0
```

Without proper isolation, these differing versions could cause conflicts. To prevent this, it's a best practice to create an isolated environment for each application that includes all its necessary dependencies.

Python addresses this with **virtual environments**. A virtual environment creates a self-contained directory containing a Python interpreter and all the packages specific to that application. This approach ensures that no external dependencies interfere with your application and that the same explicit dependency packages are consistently applied across all development, staging, and production environments. With the `requirements.txt` file and Python virtual environments, you can effectively define and isolate your Python dependencies.

### Beyond Python: Universal Isolation with Docker

What about tools that your application relies on but are outside of Python's dependencies, such as the `curl` command or other system utilities? This is where a more universal solution like **Docker containers** comes into play.

Docker allows you to package your application and all its dependencies—including system tools and configurations—into a self-contained, isolated unit called a container. This offers a highly efficient and reliable way to manage dependencies across different programming languages and environments. For the remainder of this discussion, we'll focus on a Docker-based solution.

---

### Packaging with Docker

Here's how your application code, `requirements.txt` file, and a `Dockerfile` work together to package your application and its dependencies into a Docker container:

#### 1. Application Code (`app.py`)

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')  
def welcomeToAbukiks():
    return "Welcome to Abukiks!"

if __name__ == "__main__": 
    app.run(host="0.0.0.0", debug=True)
```

#### 2. Dependencies (`requirements.txt`)

```requirements.txt
flask==2.0.0
```

#### 3. Dockerfile

```dockerfile
FROM python:3.10-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy the requirements file and install dependencies
COPY requirements.txt .
RUN pip install -r requirements.txt --no-cache-dir

# Copy the application code
COPY . .

# Define the command to run the application
CMD python app.py
```

Let's break down the `Dockerfile`:

* `FROM python:3.10-alpine`: This line creates an image based on the lightweight Python 3.10 Alpine Linux base image.
* `WORKDIR /app`: This sets the **working directory inside the container** to `/app`. This is a common and clear convention.
* `COPY requirements.txt .`: This copies the `requirements.txt` file from your local project's root directory **to the current working directory (`/app`) inside the container**.
* `RUN pip install -r requirements.txt --no-cache-dir`: This command installs all the Python dependencies listed in `requirements.txt` inside the container. The `--no-cache-dir` flag helps keep the image size smaller by preventing pip from caching downloaded packages.
* `COPY . .`: This copies all the remaining files from your local project's root directory (including `app.py`) **to the current working directory (`/app`) inside the container**.
* `CMD python app.py`: This defines the command that will run your application when the container starts.

By running the `docker build` command (e.g., `docker build -t my-flask-app .`), you create a Docker image that encapsulates your application and all its dependencies. Subsequently, running `docker run` (e.g., `docker run -p 5000:5000 my-flask-app`) will launch an instance of your application within a self-contained, isolated Docker container.

This approach ensures that your application always runs with its exact specified dependencies, regardless of the underlying system, fully embracing the 12-Factor App principle of explicitly declaring and isolating dependencies.

---

## The 12-Factor App: Scale out via the process model

### VIII. Concurrency

So, number eight in the 12-Factor App is **concurrency**. We've already discussed containerizing our application and running it as a Docker container. This executes one instance, or one process, of our application, and this instance is able to serve several users. But what happens when we have more users visiting our site? We previously talked about scaling up resources vertically by increasing the resources to the server. However, that requires taking down the server, causing downtime, and we'll ultimately hit a maximum limit on the resources that can be added to that server.

With new servers now accessible within minutes, we can provision more servers and spin up more application instances with great ease. We can then use a **load balancer** to distribute the load across these different application instances. However, for this to work as expected, we must build our application as an independent, **stateless app**. In the 12-Factor App, processes are a first-class citizen. Applications should **scale out horizontally**, not vertically, by running multiple instances of the application concurrently. The application itself should be built with this in mind. We'll discuss more about what that means for processes in the following documentation.

---

## The 12-Factor App: Execute the app as one or more stateless processes

12-Factor processes are **stateless and share nothing**. Let's see what that means. Imagine we've decided to add a new feature to our app: showing a visitor count on our website. Every time a new visitor comes to our page, we'd like to display the total visitor count. For this, we might update our code to include a `visitCount` global variable and increment it each time a request comes in:

```python
from flask import Flask

app = Flask(__name__)

visitCount = 0

@app.route('/')  
def welcomeToAbukiks():
    global visitCount
    visitCount+=1
    return "Welcome to Abukiks!"

if __name__ == "__main__": 
    app.run(host="0.0.0.0", debug=True)
```

---

### VI. Processes

This approach works well when you have one process running, because the `visitCount` is stored in the memory of that single process. But when you run multiple processes, they each have their own version of this variable stored in them. As a result, different users might see different numbers, depending on which process served them.

The same issue applies to other details, such as user session information. When a user logs into our website, we store certain session information about that user, such as where they logged in from, when their login expires, and so on. This session information is needed on the server to keep that user logged in. But if this is stored in the process memory or locally in the file system of that process, then if a future request from that user is directed to another process, the user might be considered logged out because the session information isn't available there.

Some load balancers are session-aware and can redirect users to the same process each time. This is called **sticky sessions**. However, this is still problematic if, for some reason, the process crashes, as all locally stored data will be lost. This is why 12-Factor processes are **stateless and share nothing**. Sticky sessions are a violation of the 12-Factor methodology and should never be used or relied upon.

Instead, we must not store anything in these processes. All data and session information should be stored in an **external backing service**. This way, all data is stored in a place that can be easily accessed by all processes. It doesn't matter which process a user is routed to; it's as if all requests are handled by the same process, as all processes now have access to the same set of data. This external service could be a database or a caching service like Redis.

So, we modify our code to store the visit count in a Redis database:

```python
from flask import Flask
from redis import Redis

app = Flask(__name__)
redisDb = Redis(host='redis-db-', port=6380)

@app.route('/')  
def welcomeToAbukiks():
    redisDb.incr('visitorCount')
    visitorCount = str(redisDb.get('visitorCount'), 'utf-8')
    return "Welcome to Abukiks! Visitor count: " + visitorCount

if __name__ == "__main__": 
    app.run(host="0.0.0.0", debug=True)
```

This change now allows us to run as many instances of our application as required while ensuring we store nothing locally and enabling all instances to point to the same count.