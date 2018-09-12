---
title: 'Getting an ssl certificate with Let''s Encrypt'
media_order: '2018-08-26 09_32_01-Certbot.jpg'
published: true
date: '07:23 26-08-2018'
taxonomy:
    category:
        - Infrastructure
        - Security
    tag:
        - SSL
        - Certificates
        - HTTPS
---

As of 2018 (and from a pretty long time now), SSL security for services is not an option.
Beforehand, it was not so easy for a developer to get an ssl-certificate which was valid, it was most often a self-signed certificate which was difficult to use programmatically.
Ideed, most languages and frameworks do not accept the certificate if they can't evaluate the full certifcation path (which means knowing the issuer as well as verifying the validity of the considered certificate).

To spread the usage of ssl, even for small needs, the Let's Encrypt initiative facilitates a lot the creation of valid certificates, and they have partners like [certbot](https://certbot.eff.org) which help you automate the SSL certificate creation and the SSL setup on your web server.

You can use quite easily, just go to the web site, and select your technologies, you will be able to get a **free** and **valid** certificate.

Hereafter, you can find an example with apache on Centos 7, but a lot of configurations are possible.
![](2018-08-26%2009_32_01-Certbot.jpg)

So don't wait for production day to switch on SSL mode, you can now do it from the first development day!