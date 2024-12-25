# Create-DynamoDB-Table-And-Configure-Access-with-IAM
"ChelseaFC starting XI"

![image alt]()

# Intro 

In this article, I’ll show you how to create a starting line up for football team  by going through the steps to create a “ChelseaFC starting XI” Amazon DynamoDB table, populate the table with "Chelsea's best players" and use AWS IAM to demonstrate the principles of least privilege by allowing read-only access to our table using the AWS CLI from an EC2 Instance.

# Background

# Amazon DynamoDB (DynamoDB)

DynamoDB is an AWS managed database service that provides fast and flexible storage for applications that need to store and retrieve data quickly. It is a NoSQL database, meaning it does not use the traditional SQL relational database structure. Instead, it stores data in tables with a simple key-value structure.

DynamoDB is designed to handle large amounts of data and support high levels of traffic. It automatically scales up or down to meet the needs of the application.

One of the main benefits of DynamoDB is its low latency. It is optimized for fast reads and writes, making it a good choice for applications that require real-time data access.

# AWS Identity and Access Management (IAM) Role

In IAM, a IAM role is not a user or group, and does not have its own set of credentials. It is an AWS identity with a set of permissions that you can use to grant AWS services and applications access to your resources.


# Partition Key

In Amazon DynamoDB, a partition key is a special type of primary key used to distribute data evenly across the servers in a DynamoDB table, allowing the table to scale horizontally and handle a large amount of data and traffic.

Each item in a DynamoDB table has a unique primary key, which consists of a partition key and an optional sort key, which is used to sort the data within a partition.

# Use Case

I Love watching the English Premier league, however, i have decided to create an Amazon DynamoDB table to store my all time "Chelsea's best starting XI" and give access to others to view the items in the table.

# Prerequisites

— AWS account and IAM user (Free Tier)

— A Command Line Interface (CLI)


# Step 1: Create and populate new DynamoDB table

# Create DynamoDB table

Navigate to the DynamoDB dashboard and click “Create table”, as show below.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/9635ea83fe4a7e456cfb2f16afc3d080bf181746/images/Screenshot%202024-12-25%20203009.png)


Proceed to name your DynamoDB table, then provide a Partition key and optional Sort key. Note, both keys create a composite primary key, composed of the two attributes. All data under a Partition key is sorted by the Sort key value.

For this demonstration, I have chosen “Player Position” as my Partition key and “Player name” as my Sort key because those are my favorite players.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/2d4b04507b647597e6c944c84194d87840d89d9c/images/Screenshot%202024-12-24%20110510.png)

After providing a Partition key and Sort key, continue to “Create table”.

Your table should now be created, however, you may need to wait a few minutes until the status is “Active”, as shown below.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/a675b23f01e2b322fdf65506720ff0f8701b5760/images/Screenshot%202024-12-24%20110621.png)


# Add items to DynamoDB table

Select your newly created table. On the left pane, click “Explore items”, then “Create item”.

![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/539d4733b6b1de81e2b554c375e119f9b606ec9c/images/Screenshot%202024-12-25%20204233.png) 

You can now begin populating your attributes with the corresponding values . In my example, I added  “Sanchez” as the "goalkeeper".


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/fd71832de3b00cc3a7a031cf2fb6db376c208a23/images/Screenshot%202024-12-25%20204720.png)


After adding all items to the DynamoDB table, you can preview the table and all its items, as show below. This ChelseaFootballClub table has been populated with 6 best players.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/d047c740fc1620793a2e5748d51d6730565d1ae0/images/Screenshot%202024-12-24%20110939.png)


Now that we’ve created and populated our DynamoDB table with our items, we can proceed to Step 2: IAM Role!

# Step 2: Launch EC2 with IAM role to scan DynamoDB table

# Create IAM Role to allow read-only access authorization

Navigate to the IAM dashboard and on the left plane click “Roles”, then “Create role”.


![image alt]()












