---
title: Secure Spring Boot application with Let's Encrypt
date: 2017-12-24 23:36:36
tags: technical
thumbnail: https://i.imgur.com/s5nuHij.png
---

# Introduction 

Spring boot and Let's Encrypt is both widely used in the industry. Spring Boot gives power to make a Spring-powered application easily; while Let's Encrypt offers free SSL/TLS certificates for enabling secure HTTPS connections between client and server (the most easily recoginized is that the browser will show a green padlock beside the URL).

But to integrate them has some difficulties as below:

1. Certificates offered by Let's Encrypt are valid for only 90 days; renewing process needs to be processed gradually.
2. Spring Boot needs to restart to adopt new certification. Which quite painful.
3. Let's Encrypt program offer key in PEM files, while Spring Boot supports for PKCS12 type. It is neccesary to convert certificate and private key to PKCS12.

<!-- more -->

# Require

In this tutorial, we will use the following material:

- One server running Ubuntu 16.04
- A fully registered domain name

In my case, I use Google Azure - they offer all I need.

# Design

For easy adopting Let's Encrypt with Spring Boot, NginX is used as a SSL Proxy server. 

- Spring Boot is running on port 8080 and not accessible from internet.
- Request to port 80 is redirected to port 443 by NginX.
- NginX receives all request from port 443 and redirect to port 8080.
- Let's Encrypt certificate is servered by NginX, NginX will be restarted after certification renewal is succeed.

![NginX proxy](https://i.imgur.com/DerwAZl.png)

# Step 1: Install Java (skip this part if you have Java installed)

Open the terminal, add the PPA by running:

```
sudo add-apt-repository ppa:webupd8team/java
```

Install Java 8 (you can change the package name to oracle-java9-installer for Java 9)

```
sudo apt update; sudo apt install oracle-java8-installer
```

You now can check Java version after installing the package by running:

```
javac -version
```

And now for automatically set Java enviroment variables:

```
sudo apt install oracle-java8-set-default
```

# Step 2: running a simple Spring Boot application

We will clone a simple program, which running on port 8080 [here](https://github.com/bachnxhedspi/simple-spring-boot-app).

Remote connect to your server and running:
```
git clone https://github.com/bachnxhedspi/simple-spring-boot-app
cd simple-spring-boot-app
java -jar gs-spring-boot-0.1.0.jar &
```
The Spring Boot is running successful if you see this: 

![Spring Boot running successful](https://i.imgur.com/TPXdyti.png)

You can test it again by running:
```bash
curl localhost:8080
```

It should return "Greetings from Spring Boot!".

# Step 3: install NginX

NginX installation tutorial can be found [here](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04). Or you just need to run the following command:

```bash
sudo apt-get update
sudo apt-get install nginx
```

Now allow access to port 80 and port 443 in the Azure Portal by opening Network Security Group of your Virtual Machine.

![Create a new rule](https://i.imgur.com/E40rE3f.jpg)

![Rule for allowing access to port 80 and 443](https://i.imgur.com/OQvnVBi.jpg)

![Access to the domain... NginX will show up](https://i.imgur.com/B2hLEaY.png)

We will redirect all request to our website to port 8080. Open /etc/nginx/nginx.conf and insert to server part.

```file
    server {
        ....

        # Change to your domain
        server_name springbootdemo.centralus.cloudapp.azure.com;
        
        ....

        # Redirect all request to port 8080
        location / {
		proxy_pass http://127.0.0.1:8080;
        }

        ...
```

And now restart nginx by runnning

```bash
sudo service nginx restart
```

![Redirect successful](https://i.imgur.com/UG9wSJn.png)

# Step 4: Acquire and adopt Let's Encrypt by using Certbot

[Certbot](https://certbot.eff.org/) is an easy-to-use automatic client that fetches and deploys Let's Encrypt SSL/TLS certificates for our webserver. 

Install Certbot
```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx
```
Installing certificate to NginX by running:

```
sudo certbot --nginx
```

As usual, we can running domain challenging by the above command with tls-sni-01. But currently Let’s Encrypt has currently disabled the TLS-SNI-01 challenge due to [a security report](https://community.letsencrypt.org/t/2018-01-11-update-regarding-acme-tls-sni-and-shared-hosting-infrastructure/50188). So we will implement this command instead.

```
sudo certbot --authenticator standalone --installer nginx
```

Remember to select the domain (by just press enter) and select for redirect through HTTPS (by enter 2) when being asked.

```
$ sudo certbot --authenticator standalone --installer nginx
...

Which names would you like to activate HTTPS for?
-------------------------------------------------------------------------------
1: springbootdemo.centralus.cloudapp.azure.com
-------------------------------------------------------------------------------
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel):
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for springbootdemo.centralus.cloudapp.azure.com
Waiting for verification...
Cleaning up challenges
Deployed Certificate to VirtualHost /etc/nginx/nginx.conf for set(['springbootdemo.centralus.cloudapp.azure.com'])
nginx: [error] invalid PID number "" in "/run/nginx.pid"

Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
-------------------------------------------------------------------------------
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
-------------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 2
Redirecting all traffic on port 80 to ssl in /etc/nginx/nginx.conf


```

You can check /etc/nginx/nginx.conf, it was modified by Certbot for using Let's Encrypt certificate.

And now it's done. Our website is secured by Let's Encrypt certificate

![Well done, a A grade certificate. Rated by SSL Labs](https://i.imgur.com/SYiWUwa.png)

# Step 5: Setting Let's Encrypt auto renewal

But there is no silver bullet; there is a trade-off between certificates duration and security (you can read more about it [here](https://letsencrypt.org/2015/11/09/why-90-days.html)). Its duration is only 90 days, compare to normal paid certificatis which valid for some years. But it can be solved by set a crontab to auto renew it.

```
sudo crontab -e
```
It will open the crontab of su user, insert this line below
```
30 3 * * * certbot renew --post-hook "service nginx reload"
```
It will run everyday at 3:30 AM; you can trust it because certbot will only run if the certificate is valid less than 30 days.

# Conclusion

In this tutorial, we installed a Let's Encrypt SSL/TLS certificate for securing our Spring Boot application by using NginX as a SSL proxy. If you have any questions, please look at the links below first.

Notice: for some distros (such as CentOS), [SELinux](https://en.wikipedia.org/wiki/Security-Enhanced_Linux) can cause permission denied while connecting upstream. Solution can be found [here](https://stackoverflow.com/questions/23948527/13-permission-denied-while-connecting-to-upstreamnginx).

# Reference material

http://tipsonubuntu.com/2016/07/31/install-oracle-java-8-9-ubuntu-16-04-linux-mint-18/

https://spr.com/part-1-getting-started-again-build-a-web-app-with-spring-boot/

https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04

https://certbot.eff.org/docs/