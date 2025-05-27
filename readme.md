# Cloud & Infrastructure as Service Basics

### Demo Project:
Create server and deploy application on DigitalOcean

### Technologies used:
DigitalOcean, Linux, Java, Gradle

### Project Description:

- [Setup and configure a server on DigitalOcean](#setup-and-configure-a-server-on-digitalocean)
- [Create and configure a new Linux user on the Droplet (Security best practice)](#create-and-configure-a-new-linux-user-on-the-droplet-security-best-practice)
- [Deploy and run a Java Gradle application on Droplet](#deploy-and-run-a-java-gradle-application-on-droplet)

## Setup and configure a server on DigitalOcean

 1. Create an account on Digital Ocean
 2. After logging in, go to Droplets and create a new droplet with Linux distribution.
      2.1 You will have automatically created a root user and you can connect to droplet by SSH
      2.2 The SSH key that you’ll use to connect to the droplet is the public key from your computer that you’ve added to Gitlab or Github
  3. When you create a server it’s publicly available to anyone and needs to be restricted by configuring the Firewall so that I explicitly allow access on specific ports. ( SSH Port = 22 )
      3.1 Click on the droplet name and then on Networking -> Create Firewall.
      3.2 Add a name to the firewall and for Inbound rules: SSH, PORT 22 and remove from Sources everything and add your IP address. Then click on Create Firewall.
      3.3 Click on Firewall name, click on Droplets tab and assign your droplet.
  4. To connect to the droplet from your computer copy the IP address and into your terminal type: **ssh root@IP_ADDRESS**
  5. Nothing is installed on droplet and initially type: **apt update**
  6. For example, to install Java, type: **java** and you’ll receive versions for it.
  7. Install version 8 of Java, by copying the command displayed as result.

## Deploy and run a Java Gradle application on Droplet

Prerequisites: Java is installed and now we have to build a JAR file that will be installed on the droplet so we can run the app and access it from browser.
   1. Open another terminal on your local computer and go into example project: java-react-example.
   2. Run: gradle build
   3. Now with secure copy, you’ll copy the jar file into the remote address of the droplet: **scp <file locally> <destination>** (scp build/libs/java-react-example.jar root@IP_DROPLET:/root)
   4. On the other terminal for droplet, where you’re logged in with the root user, type: ls and check that the jar file is successfully loaded.
   5. To start the application run: **java -jar java-react-example.jar**
   6. App is now running, but the port to access from browser is not opened, then copy the port that’s displayed and go to your droplet on DigitalOcean.
   7. Go to Networking -> Firewall -> Click on your firewall and add a new Custom rule: Custom, TCP, PORT_ID, from all sources: IPv4, IPv6 and Save (now application is publicly available from browser)
   8. To access from browser, copy droplet ID and in your browser type: DROPLET_ID:PORT.
   9. You can run the app in detached mode, it means you can close the terminal and the app is still running: **java -jar java-react-example.jar**
   10. To check if the process is running use: **ps aux | grep java**
   11. Using netstat, you can see the connections that have active internet connection. In case it’s not installed, you can installed it like with java.
   12. Run: **netstat -lpnt** and you’ll see the port that’s used

## Create and configure a new Linux user on the Droplet (Security best practice)

You shouldn’t give the service root user permissions, so a new user role it’s needed.
  1. On the terminal where you’re logged into the droplet with root user, type: **adduser USERNAME** and set a password.
  2. You need to allow the new user the permissions that a root user has which means you need to add it to ‘sudo’ group. Type: **usermod -aG sudo USERNAME**
 3. Switch to the new user by: **su - USERNAME**
You’ll see your name and now you’re on home directory, Also $ is for Linux user and # is for root user.
 4. To exit from this user, type: **exit**
 
If you want to ssh into the droplet, you need to add the ssh keys that you’ve added for the root user to the new created user.
  1. **ssh root@droplet_IP**
  2. Switch to the new user: **su - USERNAME**
  3. On a new tab, where you a on your local machine type: Take public ssh key: **cat ~/.ssh/id_rsa.pub** and copy it.
  4. On your terminal, for droplet, create a new folder: **mkdir .ssh**
  5.  Create a new file where you’ll provide the keys: **sudo vim .ssh/authorized_keys**
  6. Save the file.
  7. Exit again and try to ssh into your droplet and now it would be successful.
This is the best practice to use a new user for that specific service.
  
<hr/>

# Java-React Example

An example of how to use JS frontend to consume an endpoint written in Java.

## Frontend technologies

- [React](https://facebook.github.io/react/) - UI Library
- [Redux](http://redux.js.org/) - State container

## Additional information

This project is a part of a [presentation](https://docs.google.com/presentation/d/1-yZhsM43cyWWDVn6EUtK_wc39FAv-19_jwsKXlTe2o8/edit?usp=sharing)

Related projects:

- [react-intro](https://github.com/mendlik/react-intro) - Introduction to react and redux.
- [java-webpack-example](https://github.com/mendlik/java-webpack-example) - Advanced example showing how to use a module bundler in  a Java project.

Tip: [How to enable LiveReload in IntelliJ](http://stackoverflow.com/a/35895848/2284884)

<hr/>
Original project can be found here: https://github.com/pmendelski/java-react-example 
