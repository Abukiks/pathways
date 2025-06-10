# Source Control Management with GIT

## What is GIT?

GIT is a distributed version control system that allows multiple developers to work on a project simultaneously. It tracks changes in source code, enabling collaboration and maintaining a history of modifications.

## Installing GIT

To install GIT on CentOS, run the following command:

```sh
$ yum install git
```

Verify the installation by checking the GIT version:

```sh
$ git version
```

## GIT Repositories

A GIT repository is a storage space where your project files and their history are stored. Repositories can be local or remote.

### Local Workflow

1. **Initialize a GIT Repository**
   ```sh
   $ git init
   ```

2. **Check Repository Status**
   ```sh
   $ git status
   ```

3. **Add Files to Staging Area**
   ```sh
   $ git add .
   ```

4. **Commit Changes**
   ```sh
   $ git commit -m "Version 1"
   ```

5. **View Commit History**
   ```sh
   $ git log --all --graph
   ```

6. **Push Changes to Remote Repository**
   ```sh
   $ git push origin master
   ```

## GIT Remote Repository

A remote repository is a version of your project that is hosted on the internet or another network. It allows multiple users to collaborate on the project.

### Remote Workflow

1. **Add a Remote Repository**
   ```sh
   $ git remote add origin https://github.com/abukiks/app.git
   ```

2. **Push Changes to Remote Repository**
   ```sh
   $ git push -u origin master
   ```

3. **Clone a Remote Repository to Local**
   ```sh
   $ git clone https://github.com/abukiks/app.git
   ```

4. **Fetch Changes from Remote Repository**
   ```sh
   $ git fetch
   ```

5. **Pull Changes from Remote Repository**
   ```sh
   $ git pull
   ```

## GIT vs GITHUB

- **GIT** is a version control system that tracks changes in your source code.
- **GitHub** is a cloud-based hosting service that allows you to manage GIT repositories online. It provides additional features for collaboration and project management.

## GIT Workflows

1. **Install GIT**
   ```sh
   $ yum install git
   ```

2. **Initialize a GIT Repository**
   ```sh
   $ git init
   ```

3. **Configure Files to Track**
   Use `git add` to specify which files GIT should track.

4. **GIT Tracks Changes**
   Check the status of your files with `git status`.

5. **Stage Changes**
   Add changes to the staging area using `git add`.

6. **Commit Changes**
   Commit the staged changes with a descriptive message:
   ```sh
   $ git commit -m "Commit message"
   ```

### Example Workflow

1. Install GIT:
   ```sh
   $ yum install git
   ```

2. Initialize a new repository:
   ```sh
   $ git init
   ```

3. Check the repository status:
   ```sh
   $ git status
   ```

4. Add all files to the staging area:
   ```sh
   $ git add .
   ```

5. Commit the changes:
   ```sh
   $ git commit -m "Version 1"
   ```

6. View the commit history:
   ```sh
   $ git log --all --graph
   ```

7. Push the changes to a remote repository:
   ```sh
   $ git push origin master
   ```

8. Add a remote repository:
   ```sh
   $ git remote add origin https://github.com/abukiks/app.git
   ```

9. Push changes to the remote repository:
   ```sh
   $ git push -u origin master
   ```

10. Clone a remote repository:
    ```sh
    $ git clone https://github.com/abukiks/app.git
    ```

11. Fetch changes from the remote repository:
    ```sh
    $ git fetch
    ```

12. Pull changes from the remote repository:
    ```sh
    $ git pull
    ```

By following these steps, you can effectively manage your source code using GIT and collaborate with others using remote repositories like GitHub.