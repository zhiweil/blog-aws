---
title: Get Started with Keycloak
tags: [keycloak]
keywords:
summary: "A Keycloak standalone server is started in this example for you to play with."
sidebar: mydoc_sidebar
permalink: keycloak_get_started.html
folder: keycloak
---
This example is based on the Keycloak's [Get Started Guide](https://www.keycloak.org/docs/latest/getting_started/index.html). A Keycloak
docker container image is created by following the steps suggested in the document. 

The docker image created in this example is intended for practise only, so it is kept as simple as possible. If you want to 
choose a Keycloak docker image for your production environment, please refer to this 
[Docker Hub page](https://hub.docker.com/r/jboss/keycloak) for details of JBoss's official Keycloak docker image.  

## 1. Environment Setup
Please follow the following steps to set up the environment for running the all the experiments in 
Keycloak's [Get Started Guide](https://www.keycloak.org/docs/latest/getting_started/index.html).

### 1.1 Clone Github Repository
```bash
git clone git@github.com:zhiweil/keycloak-examples.git
cd keycloak-examples
```
### 1.2 Run Latest Keycloak
**This is optional.** 

The examples are based on version 4.8.3.Final. You can find the latest version of Keycloak at [this page](https://www.keycloak.org/downloads.html).
and update the following line of this [Dockerfile](https://github.com/zhiweil/keycloak-examples/blob/master/get-started/Dockerfile) to run the latest version of Keycloak.
```dockerfile
ENV KEYCLOAK_VERSION 4.8.3.Final
```

### 1.3 Build Docker Image
The following docker build command might take several minutes to run, mostly spent on downloading the latest version 
of Keycloak server ZIP file. You only need to build image once. 
```bash
cd get-started
docker build -t "zhiweil/keycloak-get-started" .
```

### 1.4 Run!
Create a containter by the Keycloak image created in the previous step by the following command.
```bash
docker run -e KEYCLOAK_USER=keymaster -e KEYCLOAK_PASSWORD=password -p 8180:8080 zhiweil/keycloak-get-started:latest
```
One thing worth mentioning is the option "-p 8180:8080", which maps docker container's 8080 port to host
machine's 8180 port. The reason is that we are going to run a demo application on port 8080 of host machine later.

If you see something like the following line from the shell terminal running the previous instruction, then Keycloak has been started successfully.
```bash
...
Keycloak 4.8.3.Final (WildFly Core 6.0.2.Final) started in 12257ms - Started 577 of 834 services (557 services are lazy, passive or on-demand)
...
```
## 2. Login via Admin Console
Open any web browser and load the following URL.
```text
http://localhost:8180/auth/admin/
```
The login page of Keycloak will be displayed. You can now use the credential created previously to login.
```text
username: keymaster
password: password
```

## 3. Excercises 
The following exercises will help you understand two basic concepts of Keycloak: realm and user. You don't need to dig 
too deep into the details at the stage. 

### 3.1 Create a New Realm
```text
Realm Name = demo
```

### 3.2 Create user
#### 3.2.1 New User
Create a user in the new realm "demo". At this stage you just need to provide "username", which is the
only compulsory field for a new user (with a red asterisk in the field name). 
```text
Username = user1

```
After clicking "Save" button, you'll see several tabs to appear. To assign a password to the user, 
you'll need to choose the "Credential" tab.

#### 3.2.2 Reset Password 
The following is a list of configuratio options to be set on the "Credentials" tab.
```text
Password: password
Password Confirmation: password
Temporary: Off
```
#### 3.2.3 User Account Service
Open a browser and load the following URL.
```text
http://localhost:8180/auth/realms/demo/account
```
The user account service console will be displayed, which is for user account management purposes.
