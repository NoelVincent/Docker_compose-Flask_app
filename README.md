# Docker compose - Running a simple Flaks Application
Here, we will be creating and building and Docker image for Flask application and running/creating the container using Docker compose.

# Docker
Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. 

Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security allow you to run many containers simultaneously on a given host. Containers are lightweight and contain everything needed to run the application, so you do not need to rely on what is currently installed on the host. You can easily share containers while you work too.
You can learn more about the same in the [website.](https://docs.docker.com/get-started/overview/)

# What Is a Container
A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.
You can learn more about the same in the [website.](https://www.docker.com/resources/what-container)


# Docker compose
Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.
You can learn more about the same in the [website.](https://docs.docker.com/compose/)


# Installing Docker
> Here I am using AWS Amazon linux server
```sh
amazon-linux-extras install docker -y
systemctl restart docker.service
systemctl enable docker.service
```
Please see the [website](https://docs.docker.com/engine/install/) for more information.

# Installing Docker compose
```sh
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
```
Please see the [website](https://docs.docker.com/compose/install/) for more information.

# Flask Application

```sh
/root/Flask/
├── Dockerfile
└── project
    ├── app.py
    └── requirements.txt
```
- Created a folder "Flask" and added the Dockerfile and added the Flask application files inside a folder called "project" just like above.

```sh
vim Flask/project/app.py 
```
```sh
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
  return "<h1><center>This is Flask Application Version 1</center></h1>"

app.run(host='0.0.0.0', port=5000)
```
```sh
vim Flask/project/requirements.txt
```
```sh
flask
```
Here, in the directory "project" we added a simple flask application and defined the dependencies(flask) in the requirements.txt file that need to run this application.

```sh
vim Flask/Dockerfile
```
```sh
FROM alpine:latest
    
RUN  mkdir /tmp/pro-flask/

WORKDIR /tmp/pro-flask/

COPY .  .

RUN apk update && apk add --no-cache python3 && apk add --no-cache py-pip

RUN pip install -r requirements.txt

EXPOSE 8081

CMD [ "python3" , "app.py"]
```

# Docker compose
> Note: The compose file should have the name and the extension as docker-compose.yml
```sh
---
version: "3"
services:
  app:
    build: /root/Flask
    volumes:
      - vol1:/tmp/pro-flask/
    networks:
      - net1
    ports:
      - "8081:5000"

volumes:
  vol1:

networks:
  net1:
```
## Execution
- Running a syntax check
```sh
docker-compose config
```
- Executing the yaml file
```sh
docker-compose up -d
```
