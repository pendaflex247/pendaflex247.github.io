---
title: AWS S3 Static Website Part 2 (without redirect)
author: Nomadstrides
date: 2023-09-30 18:55:00 +0800
categories: [Blogging, Demo]
tags: [s3 static website domain]
pin: true
---


We will be using 5 major AWS services

1. **AWS Route53:** Buy/setup a domain name.
2. **AWS Certificate Manager:** Assign SSL Certificate for the domain.
3. **AWS S3 Bucket:** Create S3 bucket with static website.
4. **AWS CloudFront:** Map CloudFront endpoint (S3-CF-Route53).
5. **AWS WAF** Wab application firewall


## Assumptions:
-You already have your domain transferred or added to route53
-Hosted zone

## Network Map

![Alt text](/assets/img/aws-s3-static-website-2/static-website-networkmap.png)

## 1. Certificate Manager

Note: for this to work with cloudfront you have to be in US-East-1 N Virgina region

![Alt text](/assets/img/aws-s3-static-website-2/20230722101944.png)


Go to certificate manager

#### request a certificate

![Alt text](/assets/img/aws-s3-static-website-2/20230722100224.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722100406.png)

##### Enter your domain 
optional: use a wildcard if you are going to use a multiple subdomains(www.netxbyeslab.click)

![Alt text](/assets/img/aws-s3-static-website-2/20230722100640.png)
![Alt text](/assets/img/aws-s3-static-website-2/20230722100854.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722102353.png)

##### Validate the Certificate 

by creating a CNAME record on route53
-click on the certificate and scrow down to Domain

![Alt text](/assets/img/aws-s3-static-website-2/20230722102640.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722102908.png)


###### Check route53 to verify the record was created successfully

![Alt text](/assets/img/aws-s3-static-website-2/20230722103036.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722103120.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722103239.png)

Now on certificate manager it should how Issued

![Alt text](/assets/img/aws-s3-static-website-2/20230722103344.png)



## 2. S3 bucket

- Create S3 bucket with the domain name 
- Upload the website content to the S3 bucket


![Alt text](/assets/img/aws-s3-static-website-2/20230722103544.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722103657.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722103742.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722103823.png)

Open the Bucket and then create 

![Alt text](/assets/img/aws-s3-static-website-2/20230722103855.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722124608.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722124712.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722124752.png)


## 3. CloudFront

#### Add a cloud front distribution

![Alt text](/assets/img/aws-s3-static-website-2/20230722124906.png)


![Alt text](/assets/img/aws-s3-static-website-2/20230722125255.png)


![Alt text](/assets/img/aws-s3-static-website-2/20230722125058.png)

##### Add viewers protocol policy

![Alt text](/assets/img/aws-s3-static-website-2/20230722125531.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722125657.png)

##### Add Web Application Firewall (WAF)

![Alt text](/assets/img/aws-s3-static-website-2/20230722125824.png)

##### Add alternative name (CNAME)

![Alt text](/assets/img/aws-s3-static-website-2/20230722131140.png)


-Test the Cloud distribution domain name to make sure it works

![Alt text](/assets/img/aws-s3-static-website-2/20230722131323.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722131644.png)

Make sure cloudfront is done deploying

![Alt text](/assets/img/aws-s3-static-website-2/20230722133225.png)



## 4. Route53

##### Create an A record

Go to Route53 Hosted zone
-Create and A record for the cloud front distribution end point
-Click on the hosted zone name
-Create an A (Alias) record 
	-Point it to the cloud front distribution

![Alt text](/assets/img/aws-s3-static-website-2/20230722133358.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722133448.png)

![Alt text](/assets/img/aws-s3-static-website-2/20230722133841.png)


#### Create a CNAME record for the subdomain (www)forwading

![Alt text](/assets/img/aws-s3-static-website-2/20230722134206.png)

Final outcome should be like this

![Alt text](/assets/img/aws-s3-static-website-2/20230722134400.png)



### Findings:

I was able to get it to work without opening any of the bucket(why?)
- So that anyone with the S3 bucket end point will not be able to access it from that link

### Observations

- The certificate record must be created in US EAST 1 for cloud front to use SSL.
- Sometime it takes time for Cloud front and the Certificate to deploy completely.
- Created a cloudfront policy from cloud from itself which was added to the S3, alternatively you can add that via s3 policy secction.
- We don't need to create multiple hosted zones.
- It took about 10min for my domain and domain redirect to work.
- If the webpage does not open, Browser cache and cookies should be cleared and then restart your browser.



