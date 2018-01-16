---
breadcrumb: PCF Services
title: Creating and Managing Service Instances
---

##<a id="overview"></a>Overview

This topic describes how developers set up, operate, and scale Amazon Web Services (AWS) resources from Pivotal Cloud Foundry (PCF) by creating and managing service instances using the Service Broker for AWS.

PCF operators must follow the instructions in [Installing the Service Broker](installation.html) to install the Service Broker for AWS before developers can use it.
During installation, operators configure which AWS services they want to make available to developers in the Marketplace.
They can also set up pre-defined service plans with specific resource configurations and securely manage their AWS credentials.

The current version of the Service Broker for AWS supports the following services:

  * Amazon Relational Database Service (RDS) for 
      - PostgreSQL
      - MySQL
      - MariaDB
      - SQL Server
      - Oracle Database
  * Amazon Aurora
  * Amazon DynamoDB
  * Amazon Simple Storage Service (S3)
  * Amazon Simple Queue Service (SQS)
  * Amazon Elastic MapReduce (EMR)

Developers create and manage service instances of the Service Broker for AWS through the Cloud Foundry Command Line Interface (cf CLI) or Apps Manager.

To perform the following procedures for creating and managing service instances, a developer must be logged in to the PCF deployment via the cf CLI.

##<a id="view"></a>View Services

1. List available Marketplace services with `cf marketplace` to show information about the Service Broker for AWS services. 
    <pre class="terminal">
    $ cf marketplace
    Getting services from marketplace in org system / space iaas-brokers as admin...
    OK
    service             plans                                      description   
    app-autoscaler      bronze, gold                               Scales bound applications in response to load   
    aws-dynamodb        standard\*                                  Create and manage Amazon DynamoDB tables   
    aws-rds-aurora      basic\*, standard\*, premium\*, enterprise\*   Create and manage AWS RDS Aurora deployments   
    aws-rds-mariadb     basic\*, standard\*, premium\*, enterprise\*   Create and manage AWS RDS MariaDB deployments   
    aws-rds-mysql       basic\*, standard\*, premium\*, enterprise\*   Create and manage AWS RDS MySQL deployments   
    aws-rds-oracle      basic\*, standard\*, premium\*, enterprise\*   Create and manage AWS RDS Oracle deployments   
    aws-rds-postgres    basic\*, standard\*, premium\*, enterprise\*   Create and manage AWS RDS PostgreSQL deployments   
    aws-rds-sqlserver   basic\*, standard\*, premium\*, enterprise\*   Create and manage AWS RDS SQL Server deployments   
    aws-s3              standard\*                                  Create and manage Amazon S3 buckets   
    aws-sqs             standard\*                                  Create and manage Amazon SQS queues   
    </pre>

    <p class="note"><strong>Note</strong>: These service plans have an associated cost. Creating a service instance incurs this cost.</p>

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

<p class="note"><strong>Note</strong>: When installing the Service Broker for AWS,
   the PCF operator sets up an IAM account and policy and provides credentials to the Service Broker.
   Developers do not need access to the credentials to create service instances.
   For information about installing, see <a href="installation.html">Installing the Service Broker</a>.</p>

###<a id="rds"></a>RDS for PostgreSQL

To create a service instance of the RDS for PostgreSQL service, use `cf create-service` to create an instance of `aws-rds-postgres` with or without custom settings.

To create an instance of `aws-rds-postgres` without custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE`. The following example creates an instance named `mypostgres1` with the `standard` plan:
<pre class="terminal">$ cf create-service aws-rds-postgres standard mypostgres1
</pre>

To create an instance of `aws-rds-postgres` with custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE` with the `-c` flag and provide custom settings for the following elements:

  * Engine Version
  * Multi-AZ
  * Storage Type
  * AllocatedStorage
  * AvailabilityZone

The following example shows the syntax for each setting. You can omit settings you don't want to explicitly set:
<pre class="terminal">$ cf create-service aws-rds-postgres basic postgresdb -c '{ "CreateDbInstance": { "EngineVersion": "9.4.1", "MultiAZ": false, "StorageType": "gp2", "AllocatedStorage": 10, "AvailabilityZone": "us-east-1a", "Tags": [{"Key": "owner", "Value": "operations"}, {"Key": "Env", "Value": "staging"} ] } }'
</pre>

###<a id="mysql"></a>RDS for MySQL

To create a service instance of the RDS for MySQL service, use `cf create-service` to create an instance of `aws-rds-mysql` with or without custom settings.

To create an instance of `aws-rds-mysql` without custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE`. The following example creates an instance named `mysqldb1` with the `standard` plan:
<pre class="terminal">$ cf create-service aws-rds-mysql standard mysqldb1
</pre>

To create an instance of `aws-rds-mysql` with custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE` with the `-c` flag and provide custom settings for the following elements:

  * Engine Version
  * Multi-AZ
  * Storage Type
  * AllocatedStorage
  * AvailabilityZone

The following example shows the syntax for each setting. You can omit settings you don't want to explicitly set:
<pre class="terminal">$ cf create-service aws-rds-mysql basic mysqldb2 -c '{ "CreateDbInstance": { "EngineVersion": "5.6.27", "MultiAZ": false, "StorageType": "gp2", "AllocatedStorage": 20, "AvailabilityZone": "us-east-1a", "Tags": [{"Key": "owner", "Value": "operations"}, {"Key": "Env", "Value": "staging"} ] } }'
</pre>

###<a id="mariadb"></a>RDS for MariaDB

To create a service instance of the RDS for MariaDB service, use `cf create-service` to create an instance of `aws-rds-mariadb` with or without custom settings.

To create an instance of `aws-rds-mariadb` without custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE`. The following example creates an instance named `mariadbdb1` with the `standard` plan:
<pre class="terminal">$ cf create-service aws-rds-mariadb standard mariadbdb1
</pre>

To create an instance of `aws-rds-mariadb` with custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE` with the `-c` flag and provide custom settings for the following elements:

  * Engine Version
  * Multi-AZ
  * Storage Type
  * AllocatedStorage
  * AvailabilityZone

The following example shows the syntax for each setting. You can omit settings you don't want to explicitly set:
<pre class="terminal">$ cf create-service aws-rds-mariadb basic mariadbdb2 -c '{ "CreateDbInstance": { "EngineVersion": "10.0.24", "MultiAZ": false, "StorageType": "gp2", "AllocatedStorage": 20, "AvailabilityZone": "us-east-1a", "Tags": [{"Key": "owner", "Value": "operations"}, {"Key": "Env", "Value": "staging"} ] } }'
</pre>

###<a id="sqlserver"></a>RDS for SQL Server

To create a service instance of the RDS for SQL Server service, use `cf create-service` to create an instance of `aws-rds-sqlserver` with or without custom settings.

To create an instance of `aws-rds-sqlserver` without custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE`. The following example creates an instance named `sqlserverdb1` with the `standard` plan:
<pre class="terminal">$ cf create-service aws-rds-sqlserver standard sqlserverdb1
</pre>

To create an instance of `aws-rds-sqlserver` with custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE` with the `-c` flag and provide custom settings for the following elements:

  * Engine Version
  * Multi-AZ
  * Storage Type
  * AllocatedStorage
  * AvailabilityZone

The following example shows the syntax for each setting. You can omit settings you don't want to explicitly set:
<pre class="terminal">$ cf create-service aws-rds-sqlserver basic sqlserverdb2 -c '{ "CreateDbInstance": { "EngineVersion": "12.00.4422.0.v1", "MultiAZ": true, "StorageType": "gp2", "AllocatedStorage": 20, "AvailabilityZone": "us-east-1a", "Tags": [{"Key": "owner", "Value": "operations"}, {"Key": "Env", "Value": "staging"} ] } }'
</pre>

<p class="note"><strong>Note</strong>: For SQL Server, setting Multi-AZ to true enables Multi-AZ database mirroring.
   For more information, see the AWS documentation, <a href="http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_SQLServerMultiAZ.html">Multi-AZ Deployments for Microsoft SQL Server with Database Mirroring</a>.</p>

###<a id="oracle"></a>RDS for Oracle Database

To create a service instance of the RDS for Oracle service, use `cf create-service` to create an instance of `aws-rds-oracle` with or without custom settings.

To create an instance of `aws-rds-oracle` without custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE`. The following example creates an instance named `oracledb1` with the `standard` plan:
<pre class="terminal">$ cf create-service aws-rds-oracle standard oracledb1
</pre>

To create an instance of `aws-rds-oracle` with custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE` with the `-c` flag and provide custom settings for the following elements:

  * Engine Version
  * Multi-AZ
  * Storage Type
  * AllocatedStorage
  * AvailabilityZone

The following example shows the syntax for each setting. You can omit settings you don't want to explicitly set:
<pre class="terminal">$ cf create-service aws-rds-oracle basic oracledb1 -c '{ "CreateDbInstance": { "EngineVersion": "12.1.0.2.v3", "MultiAZ": false, "StorageType": "gp2", "AllocatedStorage": 20, "AvailabilityZone": "us-east-1a", "Tags": [{"Key": "owner", "Value": "operations"}, {"Key": "Env", "Value": "staging"} ] } }'
</pre>

###<a id="aurora"></a>Amazon Aurora

To create a service instance of the Amazon Aurora service, use `cf create-service` to create an instance of `aws-rds-aurora` with or without custom settings.

To create an instance of `aws-rds-aurora` without custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE`. The following example creates an instance named `auroradb1` with the `standard` plan:
<pre class="terminal">$ cf create-service aws-rds-aurora standard auroradb1
</pre>

To create an instance of `aws-rds-aurora` with custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE` with the `-c` flag and provide custom settings for the following elements:

  * Multi-AZ
  * AvailabilityZone

The following example shows the syntax for each setting. You can omit settings you don't want to explicitly set:
<pre class="terminal">$ cf create-service aws-rds-aurora basic auroradb2 -c '{ "CreateDbInstance": { "MultiAZ": false, "AvailabilityZone": "us-east-1a", "Tags": [{"Key": "owner", "Value": "operations"}, {"Key": "Env", "Value": "staging"} ] } }'
</pre>

###<a id="dynamodb"></a>Amazon DynamoDB

You can create and add items to NoSQL tables with DynamoDB. You can do this programmatically or through the [AWS DynamoDB console](https://console.aws.amazon.com/dynamodb/home?region=us-east-1). Pivotal recommends creating an IAM user at bind time and configuring access to a set of prefixed tables for operations such as creating a table and adding an item. 

To create a service instance of the AWS DynamoDB service, use `cf create-service` to create an instance of `aws-dynamodb`.

To create an instance of `aws-dynamodb`, use `cf create-service SERVICE PLAN SERVICE-INSTANCE`. The following example creates an instance named `mydynamodb` with the `standard` plan:
<pre class="terminal">$ cf create-service aws-dynamodb standard MYDYNAMODB</pre>

Binding an app to a DynamoDB service instance creates an IAM User that can create tables with a prefix corresponding to the guid of the service instance. You can find the credentials to create DynamoDB tables programatically in the `VCAP_SERVICES` environment variable of the app that service is bound to, along with the table prefix and region. For example:

<pre class="json">
{
 "VCAP_SERVICES": {
  "aws-dynamodb": [
   {
    "credentials": {
     "access_key_id": "AKIAIOSFODNN7EXAMPLE",
     "prefix": "0110c45b-e0be-4b05-8aa5-7bafd5866dff_",
     "region": "us-east-1",
     "secret_access_key": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
    },
    "label": "aws-dynamodb",
    "name": "mydynamodb",
    "plan": "standard",
    "provider": null,
   }
  ]
 }
}
</pre>

The tables you create are only accessible from the app that service is bound to. Each bind creates an IAM user with full DynamoDB permissions on only the prefixed tables, for example `arn:aws:dynamodb:REGION:ACCOUNT_ID:table/PREFIX_*`.

###<a id="s3"></a>Amazon S3

To create a S3 bucket, use `cf create-service` to create an instance of `aws-s3` with or without custom settings.

To create an S3 bucket without custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE`. The following example creates an instance named `bucket1` with the `standard` plan:
<pre class="terminal">$ cf create-service aws-s3 standard bucket1
</pre>

<p class="note"><strong>Note</strong>: S3 service instances use default region and tags settings configured by the PCF operator during the installation of the Service Broker for AWS.</p>

To create an S3 bucket with custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE` with the `-c` flag. For instance, you can use custom settings to create a bucket in a specific region to lower latency for users. The following example  creates a bucket in the Tokyo region with tag values k1=v1:
<pre class="terminal">$ cf cs aws-s3 standard tokyobucket -c '{ "CreateBucket": { "CreateBucketConfiguration": { "LocationConstraint": "ap-northeast-1"} }, "PutBucketTagging": { "Tagging":{ "TagSet": [ {"Key" : "k1", "Value": "v1"}]}} }'</pre>

<p class="note"><strong>Note</strong>: Developers can supply additional configuration parameters to service instances at update and bind times.</p>

###<a id="sqs"></a>Amazon SQS

To create an SQS queue, use `cf create-service` to create an instance of `aws-sqs` with or without custom settings.

To create an SQS bucket without custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE`. The following example creates an instance named `queue1` with the `standard` plan:
<pre class="terminal">$ cf create-service aws-sqs standard queue1
</pre>

<p class="note"><strong>Note</strong>: SQS queue instances use default region and property settings configured by the PCF operator during the installation of the Service Broker for AWS.</p>

To create an SQS queue with custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE` with the `-c` flag. For instance, you can use custom settings to create a queue with a specific name and maximum message size. The following example creates a queue named "kb-queue" with a 1kb max message size:

<pre class="terminal">$ cf cs aws-sqs standard kbqueue -c '{ "CreateQueue": {  "QueueName": "kb-queue", "Attributes": { "MaximumMessageSize": "1024"} } }'</pre>

###<a id="emr"></a>Amazon EMR

To create an EMR cluster, use `cf create-service` to create an instance of `aws-emr` with or without custom settings.

To create an EMR cluster without custom settings, use `cf create-service SERVICE PLAN SERVICE-INSTANCE`. The following example creates an instance named `SERVICE-INSTANCE-NAME` with the `standard` plan:

<pre class="terminal">$ cf create-service aws-emr standard SERVICE-INSTANCE-NAME;
</pre>

<p class="note"><strong>Note</strong>: EMR cluster instances use default region and property settings configured by the PCF operator during the installation of the Service Broker for AWS.</p>

Additional configuration parameters for EMR can be specified with the `-c` flag with `cf create-service`.

<pre class="terminal">$ cf create-service aws-emr standard SERVICE-INSTANCE-NAME -c config.json;
</pre>

<strong>Sample config.json file</strong>

<pre class="json">
{
  "Applications": [
    "Hadoop",
    "HBase",
    "Pig"
  ],
  "ApplicationConfigs": [{
    "Classification": "some-classification",
    "Properties": {
      "some-property-key": "some-property-value"
    },
    "Configurations": []
  }],
  "BootstrapActions": [{
    "Name": "some-bootstrap-action-name",
    "Path": "s3://some-bucket/path/to/script.sh",
    "Args": ["arg1", "arg2"]
  }],
  "Ec2KeyName": "some-ec2-keypair-name",
  "S3LogUri": "s3://some-s3-log-bucket"
}
</pre>

You can create your config.json using a number of different variables. The following table lists the **Keys** that you can use to configure your EMR cluster.

<table class="nice">
        <th>Key</th>
        <th>Type</th>
        <th>Required</th>
        <th>Description</th>
        <tr><td><strong>Applications</strong></td>
                <td>string list</td>
                <td>no</td>
                <td>List of applications to install (e.g. "Pig", "HBase"). <br><br>For more information about supported applications for EMR release versions, see <a href="http://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-release-components.html#emr-release-applications" rel="nofollow">About Amazon EMR Releases.</a>.</td>
        </tr>
        <tr><td><strong>ApplicationConfigs</strong></td>
                <td>JSON list</td>
                <td>no</td>
                <td>List of application configurations. <br><br>For more information about application configurations, see <a href="http://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-configure-apps.html" rel="nofollow">Configuring Applications.</a></td>
        </tr>
        <tr><td><strong>BootstrapActions</strong></td>
                <td>JSON list</td>
                <td>no</td>
                <td>Bootstrap actions that run before Hadoop starts and before node begins processing data. <br><br
                    For more information about Bootstrap Actions,
                    see <a href="http://docs.aws.amazon.com/emr/latest/DeveloperGuide/emr-plan-bootstrap.html" rel="nofollow">(Optional) Create Bootstrap Actions to Install Additional Software.</a>.</td>
        </tr>
        <tr><td><strong>Ec2KeyName</strong></td>
                <td>string</td>
                <td>no</td>
                <td>Name of an existing EC2 key pair that is used for SSH access.</td>
        </tr>
        <tr><td><strong>S3LogUri</strong></td>
                <td>string</td>
                <td>no</td>
                <td>Logging output S3 bucket URI.</td>
        </tr>
</table>

An example of configuring an application is shown below using HBase. To configure HBase to use an S3 bucket as a root directory, use the following snippet for the <strong>ApplicationConfigs</strong> portion of the config.json:

<pre class="json">
"ApplicationConfigs": [{
    "Classification": "hbase-site",
    "Properties": {
      "hbase.rootdir": "s3://your-hbase-root-directory-bucket"
    }
  }, {
    "Classification": "hbase",
    "Properties": {
      "hbase.emr.storageMode": "s3"
    }
  }]
</pre>

##<a id="bind"></a>Bind or Unbind a Service Instance

Binding an RDS database or S3 service instance to an app:

* Grants the app access to the RDS database or S3 bucket.
* Provides credentials in the environment variables.

Access permissions applied by the bind operation are set at the lowest level.

Run the following command to bind a service instance to an app:
<pre class="terminal">$ cf bind-service YOUR-APP YOUR-SERVICE-INSTANCE</pre>

<p class="note"><strong>Note</strong>: When binding to an RDS database, a new user and password is created for the binding.
   Oracle is an exception to this rule, where bound apps are provided with the master username and password. </p>

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

<br>For more information about Service Instances, see [Managing Service Instances](https://docs.pivotal.io/pivotalcf/2-0/devguide/services/managing-services.html).






