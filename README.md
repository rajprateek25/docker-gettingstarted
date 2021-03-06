# Docker: Getting Started Challenge
The purpose of this document is to provide a walk-through for:
- Installing Docker Desktop on Windows
- Getting acquainted with Docker
- Downloading and running a simple application in a Docker Container  

## Docker Desktop on Windows
Follow the steps on the below link to install Docker on your desktop:
- [Install Docker Desktop on Windows](https://docs.docker.com/docker-for-windows/install/ "Official Website: Docker")

### Windows Subsystem for Linux (WSL)
Apart from Microsoft Store, we may also use WSL commands in powershell or command prompt to manage Linux distributions.

- To list all the available Linux Distributions for download/install
    ```
    $ wsl --list --online
    ```
    ![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic2.JPG)
- To specify the choice of Linux Distribution to be installed as a subsystem
    ```
    $ wsl --install --distribution <Distribution Name>
    ```
- To list currently running subsystems on your system
    ```
    $ wsl --list
    ```
    ![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic4.JPG)
    
Click here to learn more about [Basic WSL Commands](https://docs.microsoft.com/en-us/windows/wsl/basic-commands)

## Getting acquainted with Docker
The following commands will help you get started with Docker:
```
$ docker pull redis
```
Pulls the latest `redis` image from docker hub to your computer
```
$ docker run redis
```
Pulls the latest `redis` image from docker hub and runs a container using this image
```
$ docker ps
```
Shows the list of running containers
```
$ docker images
```
Shows list of available docker images on your computer
```
$ docker stop/start <container_id>
```
Stop or Start a container using container ID
```
$ docker run -p 6001:6379 -d redis
```
`-p` maps host port 6001 to container port 6379.\
`-d` runs the container in detach mode.
```
$ docker run -d -p 6000:6379 --name <custom_name> redis
```
Renames the container to a custom name using `--name` switch.

## Download and Run a simple application in a Docker Container
**What is the Application about?**
> The code creates a simple 'To Do' list where you can add/remove items or also mark the entries as complete.

Running the application involves following steps:
1. Download the code.
1. Create a Docker Image using Dockerfile. 
1. Spin up a Container using Docker Image.

### Get the Application
- You can find the project source code at [Github repository](https://github.com/docker/getting-started)
- You may download the code as a zip file or clone the repository via https/ssh.

#### Download the .ZIP
You can easily download the project .zip file as shown in the screenshot below:

![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic5.JPG)

#### Clone via HTTPS
Since the GitHub repository *getting started* is public, you can simply use following command to clone the project:
```
$ git clone https://github.com/docker/getting-started.git
```
![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic6.JPG)

#### Clone via SSH
To clone the project using ssh, first create an ssh key pair and upload the public key in your GitHub account.
- Create ssh key pair using `ssh-keygen`
  ![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic7.JPG)

- You may view the public/private keys under ~/.ssh directory
  ![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic8.JPG)

- Add the public key to your GitHub Account  
  Go to **Settings** -> **SSH and GPG Keys** -> **New SSH Key** --> Paste the public key and Enter a Title -> **Add SSH Key**
  ![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic12.JPG)

You may now use ssh to clone the repository:
```
$ git clone git@github.com:docker/getting-started.git
```
![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic13.JPG)  

> TIP:
- If you face issues cloning the project via https/ssh, check your DNS settings and ensure you're not connected to a VPN. 
- After the project is cloned, you may use `code .` from within the WSL to open the project for editing in [Visual Studio Code](https://code.visualstudio.com/sha/download?build=stable&os=win32-x64-user)
  ![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic14.JPG)

### Create Container Image
We use a `Dockerfile` to build the application. A Dockerfile is a text-based script of instructions that is used to create a container image.
1. Create a file named `Dockerfile` in the same folder as the file `package.json` with the following contents.
   ``` docker 
   # syntax=docker/dockerfile:1
   FROM node:12-alpine
   RUN apk add --no-cache python2 g++ make
   WORKDIR /app
   COPY . .
   RUN yarn install --production
   CMD ["node", "src/index.js"]
   ```
1. Switch to the `app` directory and build the container image using `docker build` command
   ```
   $ docker build -t getting-started .
   ```
   `-t` flag tags the image with name **getting-started**.  \
   `.` at the end of the docker build command tells Docker to look for the Dockerfile in the current directory.

### Spin the Container
Start the container using the `docker run` command and specify the name of the image we just created:
```
$ docker run -dp 3000:3000 getting-started
```
Now, on the host machine, open your web browser to http://localhost:3000. You should see the application running.

![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic15.JPG)

Deployment is complete! Test your application by adding a few items to the list, removing items or marking items as complete.

## Leveraging Docker-Tech!
Docker has many advantages and application scenarios:

- **Light-weight**\
Docker uses OS-level virtualization unlike Virtual Machines. This allows the applications to be packaged with their dependencies and delivered in light-weight containers.

- **Reproducibility**\
Application is developed locally and shared as Docker Containers. The exact configurations of a container is written in the `DockerFile`. Sharing the DockerFile among team members ensures identical functionality and thus standardized applications.

- **Portability**\
Docker containers can run on a laptop, on physical or virtual machines in a data center, on cloud providers, or in a hybrid of these environments.

- **Isolation**\
Docker uses `namespaces` to provide isolated workspace called the container. When you run a container, Docker creates a set of namespaces for that container. These namespaces provide a layer of isolation. Each container runs in a separate namespace and its access is limited to that namespace.\
We can run multiple applications on one piece of infrastructure without interfering with each other. The dependencies or configurations in one container do not impede with other containers on the host or the host machine itself.

- **Security**\
This is an extended feature of the isolation provided by Docker. Since all the components are isolated, if one of the components is compromised, others remain unaffected.

- **CI/CD**\
With a simple `docker push` command, you can push the changes from a development to test environment. When the testing is complete, you may simply docker push the application to DockerHub repository or to the production environment, thus strengthening your CI/CD game.

## Resources

Topic | Web Link
| --- | ---
| **WSL** | https://docs.microsoft.com/en-us/windows/wsl/install
|  | https://docs.microsoft.com/en-us/windows/wsl/setup/environment#set-up-your-linux-username-and-password
|  | https://docs.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package
| **Docker** | https://docs.docker.com/get-started/overview/
|  | https://docs.docker.com/engine/reference/commandline/docker/
|  | https://docs.docker.com/compose/
| **Linux** | https://www.ssh.com/academy/ssh/keygen
|  | https://www.techradar.com/in/news/best-dns-server
|  | https://askubuntu.com/questions/130452/how-do-i-add-a-dns-server-via-resolv-conf
