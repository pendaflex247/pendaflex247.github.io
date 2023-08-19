---
title: AWS S3 Static Website
author: Nomadstrides
date: 2023-06-15 20:55:00 +0800
categories: [Blogging, Demo]
tags: [s3 static website domain]
pin: true
---

Overview:
--
1. **AWS Route53:** Buy/setup a domain name.
2. **AWS Certificate Manager:** Assign SSL Certificate for the domain.
3. **AWS S3 Bucket:** Create S3 bucket with static website.
4. **AWS CloudFront:** Map CloudFront endpoint (S3-CF-Route53).


![Secure Static Website](<https://github.com/pendaflex247/pendaflex247.github.io/blob/main/assets/img/aws-s3-static-website/Secure-Static-Website.drawio.png>)


Steps 1: Route 53
--
-Get a domain Name
	-Search for route 53
	-under domain > register domain

![Alt text](</assets/img/aws-s3-static-website/20230708195920.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708200203.png>)


![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708200258.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708200846.png>)


![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708201237.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708201350.png>)


![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708201419.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708201514.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708200639.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708201656.png>)

STEP 2: Request a Certificate 
--
**Adding SSL Certificate for Secure HTTP (HTTPS)**


-Search for certificate manager

![[Pasted image 20230708201858.png]]

![[Pasted image 20230708202023.png]]

![[Pasted image 20230708203332.png]]

![[Pasted image 20230708203404.png]]


![[Pasted image 20230708204706.png]]

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708201858.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708202023.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708203332.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708203404.png>)

**Create a CNAME record in Route 53**

-there are two ways of doing this
-Open the newly created certificate
-create records in route 53
-that will create a CNAME in route 53 for both the domain and the subdomain


![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708204735.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708205039.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708205752.png>)

**Verify that the CNAME was ADDED**

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230708210019.png>)

Note: we will come back to add A record after S3 has been created

STEPS 3:  Create S3 Bucket
--
**Create two S3 buckets with Default settings**

	- netxbytes.com
	- www.netxbytes.com

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230619150230.png>)

- upload the website details to the www.netxbytes.com 
	
![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230619150702.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230619151519.png>)

**Enable Public Settings for www.netxbytes.com bucket**

- Disable block public access
![[S3-Secure-v1/images/Pasted image 20230619151833.png]]
 ![[S3-Secure-v1/images/Pasted image 20230619151950.png]]
- Edith Bucket Policy 

![[S3-Secure-v1/images/Pasted image 20230619153701.png]]
- Add the script and make sure to add the bucket name

```json
{
  "Version":"2012-10-17",
  "Statement":[{
     "Sid":"PublicReadGetObject",
     "Effect":"Allow",
     "Principal": "*",
     "Action":["s3:GetObject"],
     "Resource":["arn:aws:s3:::example-bucket/*"]
  }]
}	
```

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230619154410.png>)

**Enable static website hosting** 
	-Properties > Static Website hosting
		-Check Enable
		-Check Static website hosting 

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230619155457.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230619155523.png>)

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230619160229.png>)

Note: if you got to the netxbytes.com it still will not work, 
-You need to add a redirect for it to work 

![[S3-Secure-v1/images/Pasted image 20230619160537.png]]


Add a redirect from the netxbytes.com to the www.netxbytes.com
	-Go to the netxbytes bucket
		-properties > static hosting 
		
![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230619161133.png>)

Final Product should be like this
	-Note: Your browser ma cache the url you you may a have to clear cache or user incognito or a different browser
	-Also i noticed the netxbytes bucket does not have to me made public for the redirect to work
	
![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230619161357.png>)

Get the Url for netxbytes.com
-It should redirect you to the www.netxbytes.com url

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230619175443.png>)

STEP 4:  Create Cloud front distribution
--
AWS CloudFront is a content delivery network (CDN) It helps deliver content, such as web pages, images, videos, and APIs, to end users with low latency and high transfer speeds.

![Alt text](<../assets/img/aws-s3-static-website/Cloud front.drawio.png>)

-Create distribution for the domain and the subdomain  www.netxbytes.com and netxbytes.com 

Note:

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230620000835.png>)

-this will enable HTTPS and Caching 
-Search for cloud front
Click create cloud front distribution

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230619222556.png>)

-The the correct  endpoint url for both bucket from S3 bucket static site under properties 

www.netxbytes.com.s3-website-us-east-1.amazonaws.com

![Alt text](<../assets/img/aws-s3-static-website/Pasted image 20230619223023.png>)

Origin
-Add Origin domain
	-http://www.netxbytes.com.s3-website-us-east-1.amazonaws.com
	
![[Pasted image 20230619224130.png]]

Default cache behavior
-Select Redirect HTTP to HTTPS

![[S3-Secure-v1/images/Pasted image 20230619224320.png]]

Web Application Firewall (WAF)
-Select Enable security protections

![[S3-Secure-v1/images/Pasted image 20230619225349.png]]

Setting
-Alternate domain name (CNAME) - optional
	-add www.netxbytes.com
-Custom SSL certificate - optional
	-Select netxbytes.com SSL
	
![[S3-Secure-v1/images/Pasted image 20230619225009.png]]

-Then Create Distribution at the bottom at the page

End Product

![[S3-Secure-v1/images/Pasted image 20230619225927.png]]

Do the same Steps for the netxbytes.com end point
	-User the website endpoint
	netxbytes.com.s3-website-us-east-1.amazonaws.com

![[S3-Secure-v1/images/Pasted image 20230619222936.png]]

![[S3-Secure-v1/images/Pasted image 20230619230256.png]]

![[S3-Secure-v1/images/Pasted image 20230619230432.png]]

![[S3-Secure-v1/images/Pasted image 20230619230524.png]]

-I will enable the WAF for production (I'm thinking since this is a redirect no need to add WAF ? I'm i wrong? )

![[S3-Secure-v1/images/Pasted image 20230619230607.png]]

![[S3-Secure-v1/images/Pasted image 20230619230924.png]]

-Then Create Distribution at the bottom at the page

End product
Both of the endpoint will be distributed to cloud front 
Traffic that is dested for www.netxbytes.com
-will be server through cloud front that is backed by the assets that are in S3


![[S3-Secure-v1/images/Pasted image 20230619231350.png]]



STEP 5: Add A record to route53
--
-Add A record for www.netxbytes.com and netxbytes.com

-**Add a record**
	-Create record

	![[Pasted image 20230619185534.png]]

-Turn on Route traffic to Alias
-Alias to S3 website end point 
-Set your current region
-Then Add the endpoint (the S3 Bucket Url  s3-website-us-east-1.amazonaws.com)
-You can turn off evaluate target health
-Then Create record
	![[S3-Secure-v1/images/Pasted image 20230619190127.png]]

Add a sub Domain
-Click on create record

![[S3-Secure-v1/images/Pasted image 20230619211823.png]]


STEPS 6: Enable HTTPS Redirect for the netxbytes.com 
--
-Edith the static website hosting and change the protocol to HTTPS

![[S3-Secure-v1/images/Pasted image 20230619232047.png]]

![[S3-Secure-v1/images/Pasted image 20230619232130.png]]
 

STEP 7: Test
--
Check the Url now to see if its HTTPS

![[S3-Secure-v1/images/Pasted image 20230619234217.png]]

