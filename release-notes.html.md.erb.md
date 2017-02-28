---
breadcrumb: PCF Services
title: Release Notes
---

###1.1.0

* IAM Policy names are configurable
* MySQL, PostgreSQL privileges are configurable by service plan
* Customizable DB Parameter Group for MySQL, MariaDB, Aurora
* Bug fix for S3 delete service instance when there are additional IAM Policies
* Bug fix for Aurora provisioning with custom VPC and SG
* Stemcell updated to 3312

###1.0.0

* Broker configuration database can be MySQL or PostgreSQL. 
* Supports service keys, App developers can get a service key for their service instance and perform permitted actions (in AppDeveloperPolicy) via the AWS CLI. These credentials may have an expiry time.
* Added ALTER privilege to RDS MySQL bind credentials
* AWS endpoint is configurable, to support C2S
* Broker Config IAM Policy name is now configurable
* Updated stemcell to 3263


###0.1.3

* Supports additional AWS RDS services: MariaDB, Aurora, SQL Server, Oracle
* Supports AWS services DynamoDB, SQS
* Supports Floating Stemcells for manageability
* Uses an AWS IAM policy (PCFInstallationPolicy) for permissions
* Quotas to limit number of instances per service
* Creating an S3 bucket supports logging to another S3 bucket
* Supports PCF 1.8.x


###0.1.2


* Supports AWS RDS for MySQL
* Supports PCF 1.7.x


###0.1.1


* Supports AWS RDS for PostgreSQL and Amazon S3
* PCF operator can create RDS service plans
* Secure management of AWS credentials
* Requires Stemcell 3146.10
* Supports PCF 1.6.x


