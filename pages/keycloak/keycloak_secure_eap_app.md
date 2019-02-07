---
title: Secure JBoss EAP Applications
tags: [keycloak]
keywords:
summary: "Configure a JBoss EAP application to be secured by Keycloak. "
sidebar: mydoc_sidebar
permalink: keycloak_secure_eap_app.html
folder: keycloak
---
This exercise is based on the Keycloak's documentation [Get Started Guide](https://www.keycloak.org/docs/latest/getting_started/index.html)

In this exercise, [Keycloak's Quick Starts Project](https://github.com/keycloak/keycloak-quickstarts) is built and
deployed on host machine, which will be secured by the Keycloak server running at http://localhost:8180. 

The Keycloak used in this exercise can be created by following this post: [Get Started with Keycloak](keycloak_get_started.html). 

As stated in Keycloak's documentation [Get Started Guide](https://www.keycloak.org/docs/latest/getting_started/index.html), 
the exercise needs the following three pre-requisites on your host machine.
* Java JDK 8
* Apache Maven 3.1.1 or higher
* Git

## 1. Install Red Hat EAP
Download the latest version of Red Hat JBoss Enterprise Application Platform (EAP) from its [Download Page](https://developers.redhat.com/products/eap/download/). 
At the time of writing, this is version 7.2.0. After downloading [ZIP file](https://developers.redhat.com/download-manager/file/jboss-eap-7.2.0.zip) and moving to
a directory which you want to run it in, run the following instruciton to unzip it.
```bash
unzip jboss-eap-7.2.0.zip
``` 
As a resutl, Red Hat EAT is extracted to folder "jboss-eap-7.2".

## 2. Install Client Adapter
Download OpendID Connect(OIDC) Client Adapter for JBoss EAP from [Keyloak Download Page](https://www.keycloak.org/downloads.html).
**You must download the corresponding client adapter for the running Keycloak version (4.8.3.Final at time of writing)
as well as the version of EAP (version 7 at the time of writing).** Running mismatched versions will not work!
```bash
cd jboss-eap-7.2
curl -s https://downloads.jboss.org/keycloak/4.8.3.Final/adapters/keycloak-oidc/keycloak-wildfly-adapter-dist-4.8.3.Final.zip -o keycloak-wildfly-adapter-dist-4.8.3.Final.zip
unzip keycloak-wildfly-adapter-dist-4.8.3.Final.zip
cd bin
./jboss-cli.sh --file=adapter-install-offline.cli
```  

## 3. Start Red Hat EAP
Run the following command to start Red Hat EAP, assuming you are still in the /bin directory under the EAP root.
```bash
./standalone.sh
```

## 4. Deploy Application Code
To clone and deploy [Keycloak's Quick Starts Project](https://github.com/keycloak/keycloak-quickstarts) on your host machine,
you run the following instructions:
```bash
git clone https://github.com/keycloak/keycloak-quickstarts
cd keycloak-quickstarts/app-profile-jee-vanilla
mvn clean wildfly:deploy
```

## 5. Register Client
Log into realm "master" by the following URL.
```text
http://localhost:8180/auth/admin/master/console
```

Create a client in realm "demo" with the following attributes.
```text
Client ID = vanilla
Client Protocol = openid-connect
Root URL = http://localhost:8080/vanilla
```
After clicking the "Save" button, you'll see a several tabs appear for the new client "Vanilla". Choose the 
"Installation" tab, and choose the option "Keycloak OIDC JBoss Subsystem XML" from the drop down named as "Format Option". 

Click the download button on the page and open the downloaded file with a text editor. Change the "WAR MODULE NAME.war" to 
"vanilla.war". The file should look like this:
```xml
<secure-deployment name="vanilla.war">
    <realm>demo</realm>
    <auth-server-url>http://localhost:8180/auth</auth-server-url>
    <public-client>true</public-client>
    <ssl-required>EXTERNAL</ssl-required>
    <resource>vanilla</resource>
</secure-deployment>
```

## 6. Register Red Hat EAP to secure Application
Go to the Red Hat EAP root directory and configure EAP to security the application we created in section 4.
This is achieved by putting the configuration code snippet into the EAP configuration file. 

The EAP configuration file resides in a sub-directory of the EAP root, which is installed in section 1. Under the EAP root 
directory, open configuration file standalone/configuration/standalone.xml by any text editor and search for the following text:
```xml
<subsystem xmlns="urn:jboss:domain:keycloak:1.1"/>
```
Replace the line above with the following XML.
```xml
<subsystem xmlns="urn:jboss:domain:keycloak:1.1">
      <secure-deployment name="vanilla.war">
                <realm>demo</realm>
                <auth-server-url>http://localhost:8180/auth</auth-server-url>
                <public-client>true</public-client>
                <ssl-required>EXTERNAL</ssl-required>
                <resource>vanilla</resource>
        </secure-deployment>
</subsystem>
```
To make the new configuration take effect, you'll need to restart EAP. Stop EAP if it is still running and then 
start it again (see section 3)

