---
title: SSL Management
weight: 5
---
***

## Aim
The aim of this document is to help you identify the SSL certificate management which may have details like different types of SSL certificates, the process of purchasing and renewing SSL certificates, and the process of monitoring SSL certificates expiry.

## What is an SSL certificate?
SSL stands for Secure Sockets Layer, a global standard security technology that enables encrypted communication between a web browser and a web server.

It is utilized by millions of online businesses and individuals to decrease the risk of sensitive information (e.g., credit card numbers, usernames, passwords, emails, etc.) from being stolen or tampered with by hackers and identity thieves. In essence, SSL allows for a private “conversation” just between the two intended parties.

To create this secure connection, an SSL certificate (also referred to as a “digital certificate”) is installed on a web server and serves two functions:
- It authenticates the identity of the website (this guarantees visitors that they’re not on a bogus site)
- It encrypts the data that’s being transmitted

## What are the different types of SSL Certificates available for purchase and how to choose which certificate to purchase?
All SSL certificates are not the same and have different purposes to be served according to the different use cases. Below are a few commonly used SSL certificates and their corresponding use cases:

### 1. Single Domain SSL Certificates:
A single-domain SSL certificate applies to one domain and one domain only. It cannot be used to authenticate any other domain, not even subdomains of the domain it is issued for.

![image-1.png](/images/image-1.png)


### 2. Wildcard SSL Certificates:
Wildcard SSL certificates are for a single domain and all its subdomains. A subdomain is under the umbrella of the main domain. Usually subdomains will have an address that begins with something other than 'www.'

For example, www.tothenew.com has a number of subdomains, including lam.tothenew.com, jira.tothenew.com, and hawk.tothenew.com. Each is a subdomain under the main tothenew.com domain.

![image-2.png](/images/image-2.png)

### 3.Multi-Domain SSL Certificates (MDC):
A multi-domain SSL certificate, or MDC, lists multiple distinct domains on one certificate. With an MDC, domains that are not subdomains of each other can share a certificate.

Multi-domain SSL certificates are also considered as Unified Communications Certificates (UCC). Any website owner can use these certificates to allow multiple domain names to get secured on a single certificate.

![image-3.png](/images/image-3.png)


## How to place an order for purchasing an SSL certificate?
1. First step throughout the SSL management process is to identify and choose the right certificate for your requirement.
2. Once you have decided and opted to purchase the right certificate, you need to place an order with the any of the SSL vendors available in the market eg. CSC Global, Digicert etc
3. You will have to inform the vendor that you wish to purchase lets say a Wildcard SSL certificate for the below domain: 

```
*.tothenew.com
```

4. You will need to generate a CSR and share it with the SSL vendor.
5. SSL vendors will validate the CSR and send the required SSL certificate after approval.
















