---
layout: post
category: blog

title: "How This Site Works"
author: domceliano
date: 2018-11-12
---

# Introduction
How does this site work? I break down the answer into four sections: how the site is hosted, how the domain service works, how the site is updated/generated, and how I track access.

# How the site is hosted
This website is hosted on Amazon S3 ("Simple Storage Service") as a static website. Because the site is static, you view generalized content and do not interact with a database or any other server-side programs -- you just retrieve files stored in my S3 folder, or "bucket". Your browser then renders the content and executes any client-side javascript I have included.

S3 buckets are private by default, but I have enabled my domcc3.com bucket to be used for web hosting, upon which it was given the address [http://domcc3.com.s3-website-us-west-2.amazonaws.com](http://domcc3.com.s3-website-us-west-2.amazonaws.com) (you can access my website there if you wish!). I have set my bucket to be stored in Oregon, in the US West region. Using Amazon S3 is, in my opinion, one of the simplest ways to host a website, and is also very cheap (a couple of dollars a month).

Without setting the correct permissions on my bucket, a user who would try to access my S3 bucket would be given the error "403 Forbidden (Access Denied)". They would know that the bucket exists and that it accepts HTTP requests (because it is responding to them), but the contents of the bucket would be inaccessible. I avoided this problem by updating the bucket policy to public, as is discussed [here](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteAccessPermissionsReqd.html). I used the same bucket policy on my domcc3.com bucket as is shown in that example.

Hosting a static website can be compared to hosting a dynamic website, which can be done, for example, by using Amazon EC2 ("Elastic Compute Cloud") or a company such as SiteGround or GoDaddy. With dynamic websites hosted on EC2, the site owner can log into a console, interact with an OS, and have a lot more flexibility over how their website works. This increased flexibility, however, normally costs more money.

# How the domain service works
To make users not have to type [http://domcc3.com.s3-website-us-west-2.amazonaws.com](http://domcc3.com.s3-website-us-west-2.amazonaws.com) every time they access my website, I use Amazon Route 53 with the registered domain of domcc3.com. Domain registration and DNS begin to get complicated quickly, but the big picture is that the domain domcc3.com is registered with my personal details through the Amazon Registrar.

In the AWS system, the Hosted Zone I created for my website has 4 Record Sets. The first is an alias of domcc3.com to domcc3.s3-website-us-west-2.amazonaws.com. The second is an alias of www.domcc3.com to domcc3.com. The third is a name server, of which there are 4 (you can look them up using "whois"). The fourth is a Start of Authority (SOA). See the AWS Hosted Zone documentation for more details of how this works.

# How the site is updated/generated
This entire website is generated using Jekyll and the Indigo theme. You can see all of its code on my [Github](https://github.com/dceliano/personal-website). Note that I have changed the theme a little to suit my needs. I add new posts to my website simply by creating new files and re-running the jekyll build command.

When I run "s3_website push", the command looks for the s3_website.yml file, which contains the name of the bucket to push to (domcc3.com in this case) as well as the ID and value of my secret key (s3_id and s3_secret). To get s3_id and s3_secret, I had to go into the IAM ("Identity and Access Management") section of the AWS Console, click on users, and select myself. IAM can get complicated because the security policies apply for all of AWS, but IAM only needs to be set up once. The keys are then permanently stored in the file Users/dom/.aws/credentials on your local machine and stay there until you delete them.

# Tracking access
I track access requests to my website. I enabled logging for my domcc3.com bucket and set the target bucket as a new bucket, logs.domcc3.com, which is not publicly accessible.

In the logs.domcc3.com bucket, a log file is created whenever an HTTP request is made to my domcc3.com bucket (according to AWS documentation, the logs are updated "periodically"). An example of a filename is "2018-07-17-18-17-44-046A38C4123C1E74", where the filename is based on the time of the request as well as a unique ID. The contents of the file are all stored in a single line, and the fields of the log file are explained [here](https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html). If I want to stop robots from accessing my site and appearing in these logs, I would need to add a robots.txt file to my root directory and hope the robots obey that file's rules.

In order to retrieve the logs, I use the "aws" command line tool to sync by typing the command "aws s3 sync s3://logs.domcc3.com ~/Documents/website_logs".