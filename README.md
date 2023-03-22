# Building-Containers-Podman

# Introduction 

Docker is a popular tool for building and running containers, but it's not the only one. Podman is a similar tool that can be used as a drop-in replacement for Docker. One advantage of Podman is that it doesn't require a daemon to run, so it can be used without root privileges.
In this tutorial, we'll build a container image using Podman and run a simple Flask application inside a container.

# Prerequisites

Before we begin, you'll need to have Podman installed on your system. You can check if Podman is installed by running the following command:
$ podman --version
If you do not have Podman installed on your system, you can follow the following steps to install Podman on your system.

# How to Install Podman

To install Podman on a Linux system, you can follow these steps:
Check if Podman is already installed on your system by running the following command:

$ podman --version

If Podman is already installed, you will see output similar to the following:

Version:      3.4.0
API Version:  3.4.0
Go Version:   go1.16.5
Built:        Thu Jul 29 18:03:23 2021
OS/Arch:      linux/amd64

2. If Podman is not already installed, you can install it using your system's package manager. On systems ((RHEL), CentOS, and Fedora (till version 21)) that use the yum package manager, you can run the following command:

$ sudo yum install podman

On systems (Debian, Ubuntu) that use the apt package manager, you can run the following command:

$ sudo apt install podman

On systems (Fedora) that use the dnf package manager, you can run the following command:

$ sudo dnf install podman

3. Once Podman is installed, you can verify that it is working correctly by running the following command:

$ podman info

This will display information about the Podman installation, including the version number, storage configuration, and network configuration. If you see output similar to the following, Podman is installed and working correctly:

host:
  BuildahVersion: 1.22.3
  Conmon:
   package: 'conmon-2.0.29-1.module+el8.5.0+8430+ea95b8e9.x86_64'
   path: /usr/bin/conmon
   version: 'conmon version 2.0.29, commit: '
...

That's it! You now have Podman installed on your system and can start building and running container images.

# Step 1: Writing the Flask Application

We'll start by writing a simple Flask application that will run inside a container. Create a new file called app.py with the following code:
from flask import Flask


app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, world!'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')


This code defines a Flask application with a single route that returns the string "Hello, world!".

# Step 2: Creating a Container Image

Next, we'll use Podman to create a container image that will run our Flask application. Create a new file called Dockerfile with the following code:
FROM python:3.9-slim-buster

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD [ "python", "./app.py" ]

This Dockerfile specifies a base image of Python 3.9 on a slim version of the Debian Buster operating system. It then sets the working directory to /app, copies requirements.txt into the container, installs the dependencies using pip, copies app.py into the container, exposes port 5000, and specifies the command to run when the container is started.

# Step 3: Building the Container Image

To build the container image, navigate to the directory containing the Dockerfile and run the following command:

$ podman build -t mycontainer .

This command tells Podman to build a new container image with the tag mycontainer using the Dockerfile in the current directory.

# Step 4: Running the Container

To run the container, run the following command:

$ podman run -p 5000:5000 mycontainer

This command tells Podman to create a new container from the mycontainer image and map port 5000 in the container to port 5000 on the host system. This will allow us to access the Flask application in a web browser by navigating to http://localhost:5000.

# Step 5: Pushing a Container Image to a Registry

Podman can also be used to push container images to a registry, such as Docker Hub or Quay.io. In this step, we'll push the mycontainer image to Docker Hub.
First, log in to Docker Hub using your Docker ID:

$ podman login docker.io

This will prompt us to enter our Docker Hub username and password.
Next, we need to tag the image with our Docker Hub username:

$ podman tag mycontainer docker.io/<username>/mycontainer

Replace <username> with your Docker Hub username.
Finally, we can push the image to Docker Hub using the podman push command:

  $ podman push docker.io/<username>/mycontainer

  This will push the image to Docker Hub, where it will be available for others to use or for us to pull down onto other systems.
  
# Conclusion

  In this tutorial, we learned how to use Podman to build and run container images without requiring Docker. We also explored how to share these images with others by pushing them to a container registry. Podman is a powerful alternative to Docker that is becoming increasingly popular, and we hope this tutorial has helped you get started with using it.
