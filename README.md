# Docker Getting Started Challenge
The purpose of this document is to provide a walk-through for:
- Installing Docker Desktop on Windows
- Getting acquainted with Docker
- Downloading and running a simple app in a Docker Container

# Contents
- Docker Desktop on Windows
 - Windows Subsystem for Linux (WSL)
- Getting acquainted with Docker
- Download and Run an app in a Docker Container
 - Get the App
  - Download .Zip
  - Clone via HTTPS
  - Clone via SSH
 - Create Container Image
 - Spin the Container

## Docker Desktop on Windows
Follow the steps on the below link to install Docker on your desktop:

[Install Docker Desktop on Windows](https://docs.docker.com/docker-for-windows/install/ "Official Website: Docker")

### Windows Subsystem for Linux (WSL)
Apart from Microsoft Store, we may also use wsl commands in powershell or command prompt to manage Linux distributions:

```
wsl --list --online
```
Lists all the available Linux Distributions for download/install.
![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic2.JPG)
```
wsl --install --distribution <Distribution Name>
```
Specify the choice of Linux Distribution to install as a subsystem
```
wsl --list
```
Lists currently running subsystems on your system
![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic4.JPG)

Click here to learn more about [Basic WSL Commands](https://docs.microsoft.com/en-us/windows/wsl/basic-commands)

## Getting acquainted with Docker
The following commands will help get started with Docker:
```
docker pull redis
```
Pulls the latest `redis` image from docker hub to your computer
```
docker run redis
```
Pulls the latest `redis` image from docker hub and runs a container using this image
```
docker ps
```
Shows the list of running containers
```
docker images
```
Shows list of available docker images on your computer
```
docker stop/start <container_id>
```
Stop or Start a container using container ID

```
docker run -p 6001:6379 -d redis
```
Maps port 6001 from the host to 6379 from the container using `-p` switch. When we hit the host port 6001, the connection is redirected to container port 6379.
```
docker run -d -p 6000:6379 --name <custom_name> redis
```
Renames the container to a custom name using `--name` switch.

## Download and Run a simple app in a Docker Container
Running an app involves following steps:
1. Download the app to Local Inventory Directory
1. Create a Docker Image using Dockerfile 
1. Spin up the Container using Docker Image

### Get the App
- You can find the project source code at [Github repository](https://github.com/docker/getting-started)
- You may choose to download the zip or clone the repository via https/ssh.

#### Download .Zip
We can easily download the project .zip file as shown in the screenshot below:
![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic5.JPG)

#### Clone via HTTPS
Since the GitHub repository `getting started` is public, we can simply use following command to clone the project to our local inventory directory:
```
git clone https://github.com/docker/getting-started.git
```
![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic6.JPG)

#### Clone via SSH
To clone the project using ssh , we first need to create an ssh key pair and upload our public key in our GitHub account.
- Create ssh key pair using `ssh-keygen`
  ![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic7.JPG)

  ![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic8.JPG)

- Add the public key to your GitHub Account

  Go to **Settings** -> **SSH and GPG Keys** -> **New SSH Key** --> Paste the public key and Enter a Title -> **Add SSH Key**

  ![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic11.JPG)

  ![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic12.JPG)


We can now use ssh to clone the repository:

```
git clone git@github.com:docker/getting-started.git
```
  ![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic13.JPG)

> TIP: 
- If you face issues cloning the project via https/ssh, check your DNS settings and ensure you're not connected to a VPN. 
- After the project is cloned, you may use `code .` from within the WSL to open the project for editing.
      ![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic14.JPG)

### Create Container Image
We need to use a `Dockerfile` to build the application. A Dockerfile is a text-based script of instructions that is used to create a container image.

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
1. Switch to the `app` directory, and build the container image using `docker build` command
    ```
    docker build -t getting-started .
    ```
    `-t` flag tags our image with the name getting-started
    
    `.` at the end of the docker build command tells Docker to look for the Dockerfile in the current directory.

### Spin the Container
Start the container using the `docker run` command and specify the name of the image we just created:
```
docker run -dp 3000:3000 getting-started
```
Now, on the host machine, open your web browser to http://localhost:3000. You should see the app

![alt text](https://objectstorage.ap-mumbai-1.oraclecloud.com/n/bm29mfisnvsu/b/docker-gettingstarted/o/pic15.JPG)

Try adding an item or two. You can mark items as complete and remove items.
