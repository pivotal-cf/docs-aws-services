---
breadcrumb: PCF Services
title: Creating and Managing Service Instances
---

##<a id="overview"></a>Overview

This topic describes how developers set up, operate, and scale Amazon Web Services (AWS) resources from Pivotal Cloud Foundry&reg; (PCF) by creating and managing service instances using the Service Broker for AWS.

PCF operators must follow the instructions in the [Installing the Service Broker](installation.html) topic to install the Service Broker for AWS before developers can use it. During installation, operators configure which AWS services they want to make available to developers in the Services Marketplace. They can also set up pre-defined service plans with specific resource configurations and securely manage their AWS credentials. 

The current version of the Service Broker for AWS supports the following services:

* RDS for PostgreSQL: Create and manage Amazon RDS database instances running several versions of PostgreSQL. 
* S3: Create and manage Amazon S3 buckets.
* RDS for MySQL: Deploy scalable MySQL deployments in minutes

Developers create and manage service instances of the Service Broker for AWS through the cf CLI. Developers cannot use Apps Manager to create or manage instances of the Service Broker for AWS because it does not support the asynchronous provisioning capability of PCF. However, they can use Apps Manager to view service information in the Marketplace Services, including service plans and plan features. 

To perform the following procedures for creating and managing service instances, a developer must be logged in to the PCF deployment via the cf CLI.

##<a id="view"></a>View Services

1. List available Marketplace Services with `cf marketplace` to show information about the Service Broker for AWS services `aws-rds-postgres` and `aws-s3`. 
<pre class="terminal">
    $ cf marketplace
    Getting services from marketplace in org system / space iaas-brokers as admin...
    OK
    service            plans                                  description   
    app-autoscaler     bronze, gold                           Scales bound applications in response to load   
    aws-rds-mysql      basic, standard, premium, enterprise   Create and manage AWS RDS MySQL deployments   
    aws-rds-postgres   basic, standard, premium, enterprise   Create and manage AWS RDS PostgreSQL deployments   
    aws-s3             standard                               Create and manage Amazon S3 buckets   
</pre>
1. View descriptions for the plans of a service with `cf marketplace -s SERVICE`.
<pre class="terminal">
    $ cf marketplace -s aws-rds-postgres
    Getting service plan information for service aws-rds-postgres as admin...
    OK

    service plan   description                                               free or paid   
    basic          For small projects and during development.                free   
    standard       For a small production database, multi-AZ, 2vCPU, 7.5GB   free   
    premium        For a mid-sized database, multi-AZ, 4 vCPU, 15GB          free   
    enterprise     For a large database, multi-AZ, 8 vCPU, 32GB              free
</pre>

##<a id="create"></a>Create Service Instances

<p class="note"><strong>Note</strong>: When <a href="installation.html">installing</a> the Service Broker for AWS, the PCF operator sets up an IAM account and policy and provides credentials to the Service Broker. Developers do not need access to the credentials to create service instances.</p>

###<a id="rds"></a>RDS for PostgreSQL

To create a service instance of the RDS for PostgreSQL service, use `cf create-service` to create an instance of `aws-rds-postgres` with or without custom settings.

To create an instance of `aws-rds-postgres` without custom settings, use `cf create-service SERVICE PLAN SERVICE_INSTANCE`. The following example creates an instance named `mypostgres1` with the `standard` plan:
<pre class="terminal">$ cf create-service aws-rds-postgres standard mypostgres1
</pre>

To create an instance of `aws-rds-postgres` with custom settings, use `cf create-service SERVICE PLAN SERVICE_INSTANCE` with the `-c` flag and provide custom settings for the following elements:
    * Engine Version
    * Multi-AZ
    * Storage Type
    * AllocatedStorage
    * AvailabilityZone

The following example shows the syntax for each setting. You can omit settings you don't want to explicitly set:
<pre class="terminal">$ cf create-service aws-rds-postgres basic postgresdb -c '{ "CreateDbInstance": { "EngineVersion": "9.4.1", "MultiAZ": false, "StorageType": "gp2", "AllocatedStorage": 10, "AvailabilityZone": "us-east-1a", "Tags": [{"Key": "owner", "Value": "operations"}, {"Key": "Env", "Value": "staging"} ] } }'
</pre>

###<a id="rds"></a>RDS for MySQL

To create a service instance of the RDS for MySQL service, use `cf create-service` to create an instance of `aws-rds-mysql` with or without custom settings.

To create an instance of `aws-rds-mysql` without custom settings, use `cf create-service SERVICE PLAN SERVICE_INSTANCE`. The following example creates an instance named `mysqldb1` with the `standard` plan:
<pre class="terminal">$ cf create-service aws-rds-mysql standard mysqldb1
</pre>

To create an instance of `aws-rds-mysql` with custom settings, use `cf create-service SERVICE PLAN SERVICE_INSTANCE` with the `-c` flag and provide custom settings for the following elements:
    * Engine Version
    * Multi-AZ
    * Storage Type
    * AllocatedStorage
    * AvailabilityZone

The following example shows the syntax for each setting. You can omit settings you don't want to explicitly set:
<pre class="terminal">$ cf create-service aws-rds-mysql basic mysqldb2 -c '{ "CreateDbInstance": { "EngineVersion": "5.6.27", "MultiAZ": false, "StorageType": "gp2", "AllocatedStorage": 20, "AvailabilityZone": "us-east-1a", "Tags": [{"Key": "owner", "Value": "operations"}, {"Key": "Env", "Value": "staging"} ] } }'
</pre>


###<a id="rds"></a>S3

To create a S3 bucket, use `cf create-service` to create an instance of `aws-s3` with or without custom settings.

To create an S3 bucket without custom settings, use `cf create-service SERVICE PLAN SERVICE_INSTANCE`. The following example creates an instance named `bucket1` with the `standard` plan:
<pre class="terminal">$ cf create-service aws-s3 standard bucket1
</pre>

<p class="note"><strong>Note</strong>: S3 service instances use default region and tags settings configured by the PCF operator during the installation of the Service Broker for AWS.</p>

To create an S3 bucket with custom settings, use `cf create-service SERVICE PLAN SERVICE_INSTANCE` with the `-c` flag. For instance, you can use custom settings to create a bucket in a specific region to lower latency for users. The following example  creates a bucket in the Tokyo region:

    cf cs aws-s3 standard tokyobucket -c '{ "CreateBucket": { "CreateBucketConfiguration": { "LocationConstraint": "ap-northeast-1"} } }'

<p class="note"><strong>Note</strong>: Developers can supply additional configuration parameters to service instances at update and bind times.</p>

##<a id="bind"></a>Bind or Unbind a Service Instance

Binding a RDS database or S3 service instance to an app grants the app access to the RDS database or S3 bucket, and provides credentials in the environment variables. The access permissions are set at the least privilege required.  

Run the following command to bind a service instance to an app:
<pre class="terminal">$ cf bind-service YOUR-APP YOUR-SERVICE-INSTANCE</pre>

Unbinding a service instance from an app removes access to the database and removes database credentials from the environment variables. 

Run the following command to unbind a service instance to an app:
<pre class="terminal">$ cf unbind-service YOUR-APP YOUR-SERVICE-INSTANCE</pre>

##<a id="delete"></a>Delete a Service Instance

<p class="note"><strong>Note</strong>: Before deleting a service instance, ensure there are no apps bound to the service instance.</p> 
Run the following command to delete a service instance: 
<pre class="terminal">$ cf delete-service YOUR-SERVICE-INSTANCE
    Really delete the service YOUR-SERVICE-INSTANCE> y
     Deleting service YOUR-SERVICE-INSTANCE in org system / space dev1 as appdev1...
     OK
     Delete in progress. Use 'cf services' or 'cf service YOUR-SERVICE-INSTANCE' to check operation status.
</pre>

##<a id="troubleshooting"></a>Troubleshooting

###Problem

As a PCF operator, when deploying the tile by clicking **Apply Changes**, I get the following error: "A client error (InvalidClientTokenId) occurred when calling the GetUser operation: The security token included in the request is invalid."

###Reason
The AWS credentials are not valid, please recheck them or recreate the credentials. 

###Problem

As a PCF operator, when deleting the product tile, I get the following error: "Can not remove brokers that have associated service instances". 

###Reason

Your service broker currently has service instances that are active. They must be deleted before the tile can be deleted. 

###Problem

As a developer, when trying to create a service, I get the following error: "Service broker error: InvalidClientTokenId: The security token included in the request is invalid." 

###Reason
The AWS credentials are not valid, please ask your PCF operator to recheck them or recreate the credentials.




