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


## What is a CSR and how can we generate a CSR?
- A certificate signing request (CSR) is one of the first steps towards getting your own SSL Certificate. 
- The CSR contains information (e.g. common name, organization, country) the Certificate Authority (CA) will use to create your certificate. 
- It also contains the public key that will be included in your certificate and is signed with the corresponding private key.
- To generate your CSR, you will need to log in to your server and use the OpenSSL software to generate a CSR and private key. Detailed instructions are as follows.

## Steps to generate a CSR

1. Below steps are just an example which demonstrates the process of generating the CSR.
2. Log in to your server, and enter the following command:
```
openssl req -nodes -newkey rsa:2048 -keyout tothenew.key -out tothenew.csr
```
3. This will generate two files: a CSR called “tothenew.csr” and a 2048-bit private key called “tothenew.key”
4. You will be prompted to enter some information for your CSR:
- Country Name (2 letter code) [AU]: IN
- State or Province Name (full name) [some state]: Uttar Pradesh
- Locality Name (e.g., city): Noida
- Organization Name (e.g., company): TO THE NEW PVT LTD.
- Organizational Unit Name (e.g., section): TO THE NEW
- Common Name (e.g., YOUR name): 
```
*.tothenew.com
```
- Email Address:  it-team@tothenew.com
5. Please enter the following “extra” attributes to be sent with your certificate request:
- A challenge password: (leave empty)
- An optional company name: (leave empty)

![newimage.png](/images/newimage.png)

6. The Common Name (CN) field, where you should enter the fully-qualified domain name of the website you require the certificate for. Note, for wildcard certificates, the CN should be in the format: 
```
*.mydomain.com
```
7. Your CSR is now generated.

## Important Notes:

- The private.key file should be kept secure (e.g., readable only by root on Linux systems).
- Removing the “-nodes” option from the OpenSSL command will request a password and
encrypt the private key.
- This can increase security, but note that the password will be required each time Apache is restarted. Hence we do not put any passwords while generating CSR.
- Extended Validation (EV) certificates require a minimum of a 1024-bit keysize if valid before 2011, and 2048-bit if they are valid into 2011. We recommend 2048-bit keysize as a minimum for all certificates.


## How to install a SSL certificate?

- There can be many use cases and they may differ depending on where you are installing the SSL certificate.
- The below example is of installing an SSL certificate on a web server that is running Apache.
- SSH on the web server and check where are the SSL files stored inside the /etc/httpd/conf/httpd.conf file.
- Below are the files that need to be changed.
```
SSLCertificateFile /etc/pki/tls/certs/star.tothenew.crt
SSLCertificateChainFile /etc/pki/tls/certs/csc_bundle.crt
SSLCertificateKeyFile /etc/pki/tls/private/star.tothenew.com.key
```
- You need to create the above files on their respective directories on the same server and paste the certificate inside “star.tothenew.crt” and paste the content of the private key you got while generating the CSR inside the “star.tothenew.com.key” file.
- Certificate chain (or Chain of Trust) is made up of a list of certificates that start from a server’s certificate and terminate with the root certificate.
- Paste the certificate chain body inside the “csc_bundle.crt”
- Note that the certificate files names can differ according to your use case.
- Once all the certificates have been created inside their respective directories, and the httpd.conf file is configured accordingly, you need to run the below command to check the httpd configuration
```
httpd -t
```
- If the Syntax is OK, then you are good to restart or reload (used when downtime cannot be afforded) your httpd server by running below command
```
/etc/init.d/httpd reload
```

## How to create a SSL certificate chain body out of the various certs in the bundle?
- We often have confusion around “how to get the certificate chain” or “what is the correct certificate chain order”. Let us shed some light on it.
- A certificate chain (SSLCertificateKeyFile) is required in the httpd.conf file. This certificate comprises the SSL certificate + TrustedSecureCertificate + USERTrustRSA + AAACertificateServies combined together in this order only.
- To create a file with the certificate chain you can run:
```
cat STAR_xpop_poptropica_com.crt TrustedSecureCertificateAuthority5.crt USERTrustRSAAAACA.crt AAACertificateServices.crt >certificate-chain.crt
```
- Your SSLCertificateKeyFile is the output file of the combination of all the above certificates.

#### You can verify if the certificate was correctly installed by clicking on the small lock in the browser window and check the SSL expiry dates.

![newimage.png](/images/newimage.png)

## SSL Management using AWS Certificate Manager

- AWS Certificate Manager (ACM) handles the complexity of creating, storing, and renewing public and private SSL/TLS X.509 certificates and keys that protect your AWS websites and applications.

- You can provide certificates for your integrated AWS services either by issuing them directly with ACM or by importing third-party certificates into the ACM management system.

- ACM certificates can secure singular domain names, multiple specific domain names, wildcard domains, or combinations of these.

- Public SSL/TLS certificates provisioned through AWS Certificate Manager are free.

- You can get more information on AWS Certificate Manager [here](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html)


## How to monitor SSL certificates expiry using AWS Lambda?

There are many online paid tools like Site24x7, Pingdom, Uptime Robot which help monitor and alert for SSL expiry but below example is for monitoring SSL expiry by running a Python function in AWS Lambda which would alert you in a timely manner and is very cost efficient.

1. Create a list of URLs you wish to monitor and put them into a text file and upload this file on to AWS S3 inside any bucket. 
2. Like in below example, we have the bucket name as “ttn-ssl-urls” & file name as “ttn-urls.txt”
3. Below function will send alerts to the defined SNS endpoints starting from 50 days to expiry and ending with a 15 days prior critical alert.
4. This function can be run everyday to identify SSL expiry if any.

```
import socket
import ssl, boto3
import re,sys,os,datetime


def ssl_expiry_date(domainname):
    ssl_date_fmt = r'%b %d %H:%M:%S %Y %Z'
    context = ssl.create_default_context()
    conn = context.wrap_socket(
        socket.socket(socket.AF_INET),
        server_hostname=domainname,
    )
    # 3 second timeout because Lambda has runtime limitations
    conn.settimeout(3.0)
    conn.connect((domainname, 443))
    ssl_info = conn.getpeercert()
    return datetime.datetime.strptime(ssl_info['notAfter'], ssl_date_fmt).date()

def ssl_valid_time_remaining(domainname):
    """Number of days left."""
    expires = ssl_expiry_date(domainname)
    return expires - datetime.datetime.utcnow().date()

def sns_Alert(dName, eDays, sslStatus):
    sslStat = dName + ' SSL certificate will be expired in ' + eDays +' days!! '
    snsSub = dName + ' SSL Certificate Expiry ' + sslStatus + ' alert'
    print sslStat
    print snsSub
    response = client.publish(
    TargetArn="arn:aws:sns:us-east-1:129158100003:TTN_MSP_Support",
    Message= sslStat,
    Subject= snsSub
    )
    
    
#####Main Section
s3 = boto3.client('s3')
client = boto3.client('sns')
def lambda_handler(event, context):
    bucket = 'fen-ssl-urls'
    key = 'fen-urls.txt'
    
    fileObj = s3.get_object(Bucket=bucket, Key=key)
    file_content = fileObj['Body'].read()
    
    for dName in file_content.splitlines():
        print("Domain name: " + dName)
        expDate = ssl_valid_time_remaining(dName.strip())
        (a, b) = str(expDate).split(',')
        (c, d) = a.split(' ')
    # Critical alerts 
        if int(c) < 15:
            sns_Alert(dName, str(c), 'Critical')
      # Frist critical alert on 20 th day      
        elif int(c) == 20:
            sns_Alert(dName, str(c), 'Critical')
    #third warning alert on 30th day      
        elif int(c) == 30:
            sns_Alert(dName, str(c), 'Warning')
    #second warning alert on 40th day
        elif int(c) == 40:
            sns_Alert(dName, str(c), 'Warning')
    #First warning alert on 50th day      
        elif int(c) == 50:
            sns_Alert(dName, str(c), 'Warning')
        else:
            print('Everything Fine. Expiry in {} days'.format(int(c)))

```

## Author of the Document

#### Name: Salman Ahsan

#### Email ID: salman.ahsan@tothenew.com

#### Mob No.: +91-9891258741













