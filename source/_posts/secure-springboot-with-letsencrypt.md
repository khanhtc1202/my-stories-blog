---
title: Secure Spring Boot application with Let's Encrypt
date: 2017-12-24 23:36:36
tags: technical
---
1. Giới thiệu spring boot + LetsEncrypt. 
2. Tại sao muốn dùng certificate của LetsEncrypt thì phải dùng proxy
3. Tạo 1 ứng dụng springboot đơn giản, chạy ở port 8080 thế nào
4. Dùng nginx để forward port ra sao
5. Dùng certbot để generate certificate

# Introduction 

Spring boot and Let's Encrypt is both widely used in the industry. Spring Boot gives power to make a Spring-powered application easily; while Let's Encrypt offers free SSL/TLS certificates for enabling secure HTTPS connections between client and server (the most easily recoginized is that the browser will show a green padlock beside the URL).

But to integrate them has some difficulties as below:

1. Certificates offered by Let's Encrypt are valid for only 90 days; renewing process needs to be processed gradually.
2. Spring Boot needs to restart to adopt new certification. Which quite painful.
3. Let's Encrypt program offer key in PEM files, while Spring Boot supports for PKCS12 type. It is neccesary to convert certificate and private key to PKCS12.

