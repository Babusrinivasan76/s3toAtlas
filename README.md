# ETL with MongoDB Atlas and AWS Glue Studio (through Pyspark scripts)

## Introduction
The amount of information is growing minute by minute and storing the volumes of data is paramount for any analytics or business intelligence. Enterprises are now generating the DataLake to consolidate all their federated data to a single location. 
	
The ETL (Extract Transform and Load) process is key functionality to having a well-structured process for the data lake. 
	
AWS provides various services for data transfer and AWS Glue is the prime service for their ETL offering. AWS Glue studio is also made available to have a graphical user interface to ease the ETL process.

In this document, we will demonstrate how to integrate MongoDB Atlas with the AWS Glue services. We will show a practical guide for loading the data from S3 through AWS Glue Crawler, Mapping, and Data Catalog services to MongoDB Atlas.
	
This can be extended to any of the source connectors of AWS GLue like CSV, XLS, Text, RDBMS, Stream data, etc.

This article is to demonstrate the capabilities of MongoDB Atlas and AWS Glue Studio Integration.


## [MongoDB Atlas](https://www.mongodb.com/atlas)

MongoDB Atlas is an all purpose database having features like Document Model, Geo-spatial , Time-seires, hybrid deployment, multi cloud services. It evolved as "Developer Data Platform", intended to reduce the developers workload on development and management the database environment. It also provide a free tier to test out the application / database features.


## [AWS Glue Studio](https://docs.aws.amazon.com/glue/latest/ug/what-is-glue-studio.html)
AWS Glue Studio is a new graphical interface that makes it easy to create, run, and monitor extract, transform, and load (ETL) jobs in AWS Glue. You can visually compose data transformation workflows and seamlessly run them on AWS Glue’s Apache Spark-based serverless ETL engine. You can inspect the schema and data results in each step of the job.

## Integration Features

With AWS Glue Studio, we can now create scripts for integrations with all the data source. In this module, we utilized the MongoDB Atlas's Spark connectors to connect to the MongoDB Atlas.
As of now there are no director connectors are available in AWS Glue Studio to connect to MongoDB Atlas. 

## Steps for Integration


### 1.Setup of AWS Network components
Setup the VPC, Subnet, NAT Gateway and VPC Endpoints

Login to the AWS console and search for VPC 
![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/04.VPC%20Search.png)
Click on the VPC and click on the "Create VPC" 
![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/05.%20create%20VPC%20.png)
Select the "VPC and more" and choose the parameters as shown.
![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/01.VPC%20Creation.png)

Click "Create".
![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/02.VPC%20Creationpng.png)



### 2.Setup of Security Group

Click on  the "Security groups" option on the left side menu.

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/03%20Security%20Group.png)

Ensure the security group routes are enabled for the accessing the database ports

### 3.Creation of MongoDB Atlas
First and foremost we need to create a MongoDB Atlas cluster (free tier is also available) for setting the MDB Atlas as either source or sink for the ETL process.

Create the MongoDB cluster
![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/06.MongoDB%20Atlas%20DB%20Creation.png)

Click "Create Database"
![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/07.%20MongoDB%20Database%20creation.png)

Click "Create Cluster"
![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/08.Create%20cluster.png)

Other Reference: 
  1. [Cloudformation Template](https://aws.amazon.com/quickstart/architecture/mongodb-atlas/)
  2. [MongoDB Atlas console](https://www.mongodb.com/docs/atlas/getting-started/)


### 4.setup the Private link

Setup the private link services to secure the connection between MongoDB Atlas and AWS VPC

Select the "Network Access" option from the side menu 

Cick on the "Private Endpoint" and select "Dedicated Cluster"

Select the "Add Private Endpoint" and follow the instruction to create the endpoint. Note, there are some AWS CLI commands to be executed for this creation
![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/09.create%20privatelink.png)

Ensure the Private Endpoint is created successfully, before moving to the next step.

Reference: [Link](https://www.mongodb.com/docs/atlas/security-cluster-private-endpoint/)


### 5.Upload the sample JSON file to S3 bucket

Upload the sample [airport.json](https://github.com/Babusrinivasan76/atlasgluestudiointegration/tree/main/images/VPC%20Creation#:~:text=1%20minute%20ago-,airports.json,-Add%20files%20via) file to the S3 bucket
![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/13.S3%20upload.png)

### 6.Setup the AWS Glue Catalog

#### Create the connection to the S3 bucket

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/14.AWS%20Glue%20Connection.png)

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/15.AWS%20Glue%20Connection.png)

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/16.AWS%20Glue%20Connection.png)


#### Validate the connection by clicking the "Test Connection"


![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/17.%20Test%20AWS%20Glue%20Connection.png)


#### Create a crawler that runs on demand to pull the data from S3 to Glue Database


![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/18.%20Create%20Crawler.png)

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/19.%20Create%20Crawler.png)

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/20.create%20crawler.png)

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/21.create%20crawler.png)

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/22.create%20crawler.png)

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/23.create%20crawler.png)

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/24.create%20crawler.png)

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/25.create%20crawler.png)



#### Run the crawler and validate the database and table after successful completion of the crawler 

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/26.run%20crawler.png)

### 7.Create a Glue Studio Job and run

 Click on the Job from the menu and select "Pyspark Scripts"

 Click "Create Job"

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/28.create%20a%20job.png)


Copy the Code from the [link](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/pyspark_s3tomdb.py) and paste 

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/29.copy%20the%20code.png)


Configure the parameters

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/30.update%20the%20job%20details.png)

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/31.update%20the%20job%20details.png)

Save the job and click "Run"

Ensure the job ran successfully. You can refer the logs for the troubleshooting


### 8.Validate the Data in MongoDB Atlas

Validate the S3 data are created as a document in MongoDB Atlas

![](https://github.com/Babusrinivasan76/atlasgluestudiointegration/blob/main/images/VPC%20Creation/32.validat%20the%20MongoDB%20data.png)


## Summary

Hope this technical guide helped you in migrating the data into the MongoDB Atlas cluster using the AWS Glue services.

This solution can be extended to any of the AWS Glue compatible source data , and the same can be migrated to MongoDB Atlas.

For any assistance please reach out to partner@mongodb.com
