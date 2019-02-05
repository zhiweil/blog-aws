---
title: Get Started
tags: [getting_started, keycloak]
keywords:
summary: "A Keycloak standalone server is started in this example for you to play with."
sidebar: mydoc_sidebar
permalink: mydoc_keycloak_get_started.html
folder: mydoc
---
This example is based on the Keycloak's [Get Started Guide](https://www.keycloak.org/docs/latest/getting_started/index.html). A Keycloak
docker container image is created by following the steps suggested in the document. 

The docker image created in this example is intended for practise only, so it is kept as simple as possible. If you want to 
choose a Keycloak docker image for your production environment, please refer to this 
[Docker Hub page](https://hub.docker.com/r/jboss/keycloak) for details of JBoss's official Keycloak docker image.  

## Environment Setup
Please follow the following steps to set up the environment for running the all the experiments in 
Keycloak's [Get Started Guide](https://www.keycloak.org/docs/latest/getting_started/index.html).

### Clone Github Repository
```bash
git clone git@github.com:zhiweil/keycloak-examples.git
cd keycloak-examples
```
### Run Latest Keycloak
**This is optional.** 

The examples are based on version 4.8.3.Final. You can find the latest version of Keycloak at [this page](https://www.keycloak.org/downloads.html).
and update the following line of this [Dockerfile](https://github.com/zhiweil/keycloak-examples/blob/master/get-started/Dockerfile) to run the latest version of Keycloak.
```dockerfile
ENV KEYCLOAK_VERSION 4.8.3.Final
```

### Build Docker Image
The following docker build command might take several minutes to run, mostly spent on downloading the latest version 
of Keycloak server ZIP file. You only need to build image once. 
```bash
cd get-started
docker build -t "zhiweil/keycloak-get-started" .
```

### Run!
Create a containter by the Keycloak image created in the previous step by the following command.
```bash
docker run -e KEYCLOAK_USER=keymaster -e KEYCLOAK_PASSWORD=password -p 8080:8080 zhiweil/keycloak-get-started:latest
```

If you see something like the following line from your favorite shell terminal, then Keycloak has been started successfully.
```bash
...
Keycloak 4.8.3.Final (WildFly Core 6.0.2.Final) started in 12257ms - Started 577 of 834 services (557 services are lazy, passive or on-demand)
...
```
## Login via Keycloak's Web Client
Open any web browser and load the following URL.
```text
http://localhost:8080/auth/admin/
```
The login page of Keycloak will be displayed. You can now use the credential created previously to login.
```text
username: keymaster
password: password
```
