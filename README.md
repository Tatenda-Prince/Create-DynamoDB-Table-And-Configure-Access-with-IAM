# Create-DynamoDB-Table-And-Configure-Access-with-IAM
"ChelseaFC starting XI"


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/acec58ab70d77e50f183e4682a0fb85cf896d34a/images/Screenshot%202024-12-26%20145454.png)


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

Proceed to select “AWS service”. Our use case will involve using an EC2 Instance to access our DynamoDB table, so we will select “EC2”, then click “Next”.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/1b60f9c456b4db76e2089f6208f1bba791e92d3a/images/Screenshot%202024-12-24%20111234.png)


Now let’s search through the AWS managed IAM policies. These policies cover common use cases and are provided by AWS in your account. Search “DynamoDB”, select the “DynamoDBReadOnlyAccess” policy, then click “Next”.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/ffb828487831205c1012c0f27b94207b0e4a515b/images/Screenshot%202024-12-24%20111259.png)



The policy below, written in JSON, “Allows” the assuming of the role “Action” from the EC2 Instance “Service”. Proceed by naming your IAM role, then clicking “Create role”.



![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/d133ca79e5bcc36a25ffa5195699c4081cd447b2/images/Screenshot%202024-12-24%20111401.png)


# Launch EC2 Instance

We need to create an EC2 Instance which we will connect into to use the AWS CLI to scan our DynamoDB table.

Let’s navigate to the EC2 dashboard and click “Launch instance”, as seen below.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/cc0848484d005ff748da5c0a34d249f5690eb36c/images/Screenshot%202024-12-24%20111552.png) 


You should now see your Amazon EC2 configuration options page. Note, most of the configurations will remain at their default state to launch our required EC2 Instance.

We will select the “Amazon Linux 2 AMI”, as seen below.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/5539937b6e1178db303d292e6c4d3219925cffe7/images/Screenshot%202024-12-25%20210657.png)

Proceed to the Instance Type options —

We will choose the “t2.micro” which is part of the AWS free tier.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/609f3822b36b6c9de736dd13d6d8977b2aabec0f/images/Screenshot%202024-12-25%20210703.png)


Proceed to the Key pair option —

Click on the “Create new key pair” to create a new key pair, then enter your desired key pair name. Select “RSA” for key pair type and “.pem” for private key file format.

Click on “Create key pair”, as show below. The “.pm” file should automatically start downloading on your local system. Locate the file after the download is complete and store it in a safe directory. Later, we can use this key pair to connect to our EC2 Instance through ssh.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/12f24cf58657b43a1a29754b40b6db2d394ad366/images/Screenshot%202024-12-24%20111740%20-%20Copy.png)


Continue to the Network settings —

We will use our default VPC. Also, we will keep “Auto-assign public IP” enabled, as this allows our EC2 Instance to automatically receive a public IP address to enable it to connect to the internet upon launch.

We will leave these setting in the default state, as seen below.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/7c1adaeacd465069c18a9774412f429e4c139627/images/Screenshot%202024-12-24%20111814.png)


Continue to the Firewall (Security Group) settings —

We will allow “SSH” traffic to enable us to securely connect to our EC2 Instance and also “HTTPS” and “HTTP” so we can send and received on our browser over the internet.

Note — Allowing from “Anywhere” is a security risk, however for this article, we will allow it for demonstration sake.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/125efdd7dc8908f86c0895eebcfc0e628eb4cecc/images/Screenshot%202024-12-24%20111837.png)


Continue to the “Summary”, then click “Launch instance”.


# Attach IAM Role to EC2 Instance

We now need to attach our DynamoDB read-only access IAM role to our EC2 to allow us to scan our DynamoDB table. Proceed by navigating to your EC2 Instance and selecting it. Click “Actions”, “Security”, then “Modify IAM role”, as shown below.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/395d0f7059e596338fbc98d93882b011273dc3e3/images/Screenshot%202024-12-24%20112111.png)


Choose the IAM role previously created, then “Update IAM role”.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/74fea117624d09d20ad547032b4e81a056355887/images/Screenshot%202024-12-24%20112123.png)


We’ve now launched our EC2 Instance with an IAM role authorizing read-only access to our DynamoDB table. Let’s proceed to Step 3: Scanning the DynamoDB table!


# Step 3: Verifying scanning of our DynamoDB table

# Connect into EC2 Instance

Navigate to the EC2 dashboard, then select your EC2 Instance.

Note, there are two ways to connect into your EC2 Instance —

EC2 Instance Connect

SSH from your local machine

I will show you how we can connect using both ways:

Connecting using “EC2 Instance Connect”— Select your new EC2 Instance, then click “Connect” on the top right of the pane. Choose the “EC2 Instance Connect” tab, then click “Connect”, as show below.

![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/7ee1df4e0c168ed6e363bcca30ee38178cbcefa8/images/Screenshot%202024-12-24%20112216.png)


Using either way, after connecting to your EC2 Instance, your display should be similar to the one below.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/d54b63faefd750ef7a1d08cfa700a0c9a3e7b51c/images/Screenshot%202024-12-25%20212328.png)


# Scan DynamoDB Table


We can now verify that we can scan the DynamoDB table, which assures us that we have read access.

To scan your DynamoDB table, run the AWS CLI command below with the name of your DynamoDB table —

aws dynamodb scan --table-name <name_of_dynamodb_table> --region us-east-1


If the command ran successfully without any errors, the items in your table should be displayed, as show below.


![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/95a23a489f7f0c45333c474def4c7695fad383d6/images/Screenshot%202024-12-24%20112551.png)


# Success!
You were able to scan your DynamoDb table and read the items stored in it! Now we should validate our read-only access permissions in Step 4!


# Step 4: Validate inability to write item to DynamoDB table

We shouldn’t be able to write to our DynamoDB table based on our IAM Role which allows read-only access from our EC2 Instance. Let’s test this out by attempting to write a new item to our table.

Run the following command to attempt to write an item to our DynamoDB table —

aws dynamodb put-item --table-name <table_name> --item '{"<partition_key>": {"S": "<value>"},"<sort_key>": {"S": "<value>"}}' --region us-east-1


You should receive an “AccessDeniedException” error, stating that we are not “authorized to perform” put-item to write to our DynamoDB table, as shown below.

![image alt](https://github.com/Tatenda-Prince/Create-DynamoDB-Table-And-Configure-Access-with-IAM/blob/e481d097f770b27e8ddf7dedafb32eb969da308a/images/Screenshot%202024-12-24%20115110.png)

# Congratulations!

You’ve just become a “football Coach” You’ve successfully created an Amazon DynamoDB table and configured read-only authorization access from EC2 Instances using IAM!































