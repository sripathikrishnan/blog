---
layout: post
title:  "Don't store AWS Access Keys in code -- use Instance Profile Roles instead"
date:   2014-01-22 10:20:50
categories: security
---

A common use case is calling an AWS API from an EC2 instance using a library like boto. For example, you may want to upload files to S3 from an EC2 machine. To do so, you require AWS access keys and secret keys..

The usual approach is to ask Pradeep to generate keys, and then store these keys in settings files. This is a bad practice, because anyone who has access to the source code can now make API calls directly. 

Solution :
1. Create a unique IAM role for your application. Example : securecpa_webserver_dev or lithium_jobserver_prod
2. Assign this role to the EC2 instance when you boot the machine.
3. When you use boto, don't pass the credentials. boto is intelligent enough to find the temporary credentials.

Wrong : boto.connect_s3(settings.access_key, settings.secret_accesskey)
Right   : boto.connect_s3()

As a process, Pradeep and team will always do steps 1 and 2 for you. The role initially will not have any permissions. As a developer, you must request specific permissions you want for the role.

Appendix :
* Instance Profile Documentation - http://docs.aws.amazon.com/IAM/latest/UserGuide/role-usecase-ec2app.html
* How Boto searches for credentials - https://github.com/boto/boto/tree/develop/boto/core