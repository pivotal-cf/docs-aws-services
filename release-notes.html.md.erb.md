---
breadcrumb: PCF Services
title: Release Notes
---

###1.4.3 Release date: Sept. 29 2017

* Fixed Service Broker deployment issue when using MySQL v5.6.x

###1.4.2

Please note: This version requires Go Buildpack v1.8.3 or later and Stemcell 3363.29 or later.

* Fixed default MySQL privileges and change grants on other MySQL compatible databases
* Fixes for C2S (Certificate, Regions)
* Added region for GovCloud
* Updated Regions dropdown for new regions

###1.4.0

Please note: This version requires Go Buildpack v1.8.3 or later and Stemcell 3363.25 or later.

* The service broker can now use an HTTP/HTTPS Proxy, configured in Ops Manager settings
* Encryption can be enforced on S3 buckets through a policy setting
* Updated Golang version to 1.8
* Upgraded to the latest AWS Golang SDK v1.8.19
* Updated to the 3363 Stemcell
* Fixed the default custom privileges on PostgreSQL service plans for upgrades

###1.3.0

* Fixed the default custom privileges on PostgreSQL service plans for new deployments; for upgrades you should set the User Privileges field to ALL. 
* Added a policy setting to enforce TLS between app and RDS database
* Service keys work for S3 services
* Note: Please add  "iam:ListUserPolicies" to the PCFInstallation IAM Policy or copy the new policy from the Installation section

###1.2.0

* Upgrade to the latest AWS Golang SDK v1.7.1
* Added C2S support using Partitions and Service identifiers
* Note: for PostgreSQL service plans, the User Privileges field must be set to ALL or the "cf bind" will fail with the error: "error: pq: invalid privilege type SELECT for database"

###1.1.0

* IAM Policy names are configurable
* MySQL, PostgreSQL privileges are configurable by service plan. Note: for PostgreSQL service plans, the User Privileges field must be set to ALL or the "cf bind" will fail with the error: "error: pq: invalid privilege type SELECT for database"
* Customizable DB Parameter Group for MySQL, MariaDB, Aurora
* Bug fix for S3 delete service instance when there are additional IAM Policies
* Bug fix for Aurora provisioning with custom VPC and SG
* Stemcell updated to 3312

###1.0.0

* Broker configuration database can be MySQL or PostgreSQL
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


