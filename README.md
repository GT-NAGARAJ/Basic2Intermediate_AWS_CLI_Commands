# Basic2Intermediate_AWS_CLI_Commands
Basic2Intermediate_AWS_CLI_Commands

Comprehensive Notes on Basic to Intermediate AWS CLI Commands for AWS Solution Architects
This document provides detailed notes on essential AWS Command Line Interface (CLI) commands for AWS Solution Architects, covering basic and intermediate levels. The notes are organized into two main sections: Basics Commands and Intermediate Commands, each including must-know commands and service-specific commands for EC2, S3, IAM, DynamoDB, and additional services like VPC and RDS. Each command includes an explanation of its purpose, options, and two practical examples. These commands are sourced from reliable references, including official AWS documentation and trusted guides, ensuring relevance for Solution Architects.
Basics Commands
These commands are foundational for managing AWS services and are essential for initial setup and core operations.
1. General Configuration
Must-know Command: aws configure

Explanation: Configures AWS CLI with credentials (access key, secret key), default region, and output format (e.g., json, table). It creates or updates the ~/.aws/credentials and ~/.aws/config files, enabling authenticated access to AWS services.
Options:
--profile: Specifies a named profile for multiple users or environments (e.g., --profile dev).
No additional options are typically needed for basic setup.


Examples:
aws configure: Prompts for access key, secret key, region (e.g., us-east-1), and output format (e.g., json).
aws configure --profile dev: Sets up a profile named "dev" with specific credentials and region.



2. EC2 (Elastic Compute Cloud)
Must-know Commands:

aws ec2 describe-instances

Explanation: Retrieves details about EC2 instances, such as instance IDs, types, states, and tags. Useful for monitoring and auditing your EC2 environment.
Options:
--instance-ids: Filters by specific instance IDs (e.g., i-1234567890abcdef0).
--query: Extracts specific fields using JMESPath (e.g., Reservations[*].Instances[*].InstanceId).
--output: Sets output format (e.g., table, json, text).


Examples:
aws ec2 describe-instances --query 'Reservations[*].Instances[*].{ID:InstanceId,State:State.Name}' --output table: Lists instance IDs and their states in a table format.
aws ec2 describe-instances --instance-ids i-1234567890abcdef0 --query 'Reservations[*].Instances[*].{ID:InstanceId,Type:InstanceType,State:State.Name}' --output table: Shows detailed info for a specific instance.




aws ec2 run-instances

Explanation: Launches new EC2 instances with specified parameters, such as Amazon Machine Image (AMI), instance type, and security groups.
Options:
--image-id: Specifies the AMI ID (e.g., ami-0c55b159cbfafe1f0 for Amazon Linux 2).
--instance-type: Defines the instance type (e.g., t2.micro).
--count: Number of instances to launch.
--key-name: Key pair for SSH access.
--security-group-ids: Security groups to associate.
--subnet-id: Subnet for the instance.


Examples:
aws ec2 run-instances --image-id ami-0c55b159cbfafe1f0 --count 1 --instance-type t2.micro --key-name YourKeyPair --security-group-ids sg-12345678: Launches one t2.micro instance.
aws ec2 run-instances --image-id ami-0c55b159cbfafe1f0 --count 2 --instance-type t3.small --key-name YourKeyPair --security-group-ids sg-12345678 --subnet-id subnet-12345678: Launches two t3.small instances in a specific subnet.




aws ec2 terminate-instances

Explanation: Terminates specified EC2 instances, stopping and removing them from your account.
Options:
--instance-ids: IDs of instances to terminate (e.g., i-1234567890abcdef0).


Examples:
aws ec2 terminate-instances --instance-ids i-1234567890abcdef0: Terminates a single instance.
aws ec2 terminate-instances --instance-ids i-1234567890abcdef0 i-0987654321fedcba: Terminates multiple instances.





3. S3 (Simple Storage Service)
Must-know Commands:

aws s3 ls

Explanation: Lists all S3 buckets in your account or objects within a specific bucket. Displays creation dates for buckets or object details.
Options:
--recursive: Lists all objects in a bucket recursively.
Path argument (e.g., s3://my-bucket): Specifies a bucket or prefix.


Examples:
aws s3 ls: Lists all buckets with their creation dates.
aws s3 ls s3://my-bucket --recursive: Lists all objects in my-bucket, including nested folders.




aws s3 mb

Explanation: Creates a new S3 bucket with a unique name. Bucket names must be globally unique across all AWS accounts.
Options:
--region: Specifies the region (e.g., us-west-2). Defaults to the configured region if omitted.


Examples:
aws s3 mb s3://my-new-bucket: Creates a bucket in the default region.
aws s3 mb s3://my-new-bucket --region us-west-2: Creates a bucket in us-west-2.




aws s3 cp

Explanation: Copies files between local storage and S3 or between S3 locations. Supports uploading and downloading.
Options:
--recursive: Copies directories recursively.
Source and destination paths: Local (e.g., file.txt) or S3 (e.g., s3://my-bucket/file.txt).


Examples:
aws s3 cp localfile.txt s3://my-bucket/path/to/file.txt: Uploads localfile.txt to S3.
aws s3 cp s3://my-bucket/path/to/file.txt localfile.txt: Downloads file.txt from S3.




aws s3 rm

Explanation: Deletes objects from an S3 bucket.
Options:
--recursive: Deletes all objects under a prefix.


Examples:
aws s3 rm s3://my-bucket/path/to/file.txt: Deletes a single file.
aws s3 rm s3://my-bucket/path/ --recursive: Deletes all objects under path/.





4. IAM (Identity and Access Management)
Must-know Commands:

aws iam create-user
Explanation: Creates a new IAM user for programmatic or console access.
Options:
--user-name: Specifies the user name (e.g., newuser).
--tags: Adds metadata tags (e.g., Key=Department,Value=Engineering).


Examples:
aws iam create-user --user-name newuser: Creates a user named newuser.
`aws iam create-user --user-name newuser --tags Key=Department,Value=Engineering  - Explanation: Attaches a managed policy to an IAM user, granting specific permissions.


Options:
--user-name: Specifies the user.
--policy-arn: ARN of the policy to attach (e.g., arn:aws:iam::aws:policy/AmazonS3FullAccess).


Examples:
aws iam attach-user-policy --user-name newuser --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess: Grants S3 full access to newuser.
aws iam attach-user-policy --user-name newuser --policy-arn arn:aws:iam::aws:policy/IAMUserChangePassword: Allows newuser to change their password.





5. DynamoDB
Must-know Commands:

aws dynamodb list-tables

Explanation: Lists all DynamoDB tables in your account.
Options:
--exclusive-start-table-name: Starts listing from a specific table for pagination.
--limit: Limits the number of tables returned.


Examples:
aws dynamodb list-tables: Lists all tables.
aws dynamodb list-tables --exclusive-start-table-name MyTable --limit 10: Lists up to 10 tables starting from MyTable.




aws dynamodb create-table

Explanation: Creates a new DynamoDB table with defined attributes, key schema, and throughput settings.
Options:
--table-name: Specifies the table name.
--attribute-definitions: Defines attributes (e.g., AttributeName=Id,AttributeType=S for string).
--key-schema: Specifies the primary key (e.g., AttributeName=Id,KeyType=HASH).
--provisioned-throughput: Sets read/write capacity units.


Examples:
aws dynamodb create-table --table-name MyTable --attribute-definitions AttributeName=Id,AttributeType=S --key-schema AttributeName=Id,KeyType=HASH --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5: Creates a table with a simple primary key.
aws dynamodb create-table --table-name MyTable --attribute-definitions AttributeName=Id,AttributeType=S AttributeName=SortKey,AttributeType=S --key-schema AttributeName=Id,KeyType=HASH AttributeName=SortKey,KeyType=RANGE --provisioned-throughput ReadCapacityUnits=10,WriteCapacityUnits=10: Creates a table with a composite key.




aws dynamodb put-item

Explanation: Inserts or updates an item in a DynamoDB table.
Options:
--table-name: Specifies the table.
--item: JSON-formatted item data (e.g., {"Id":{"S":"123"},"Name":{"S":"John"}}).


Examples:
aws dynamodb put-item --table-name MyTable --item '{"Id":{"S":"123"},"Name":{"S":"John Doe"}}': Adds a new item.
aws dynamodb put-item --table-name MyTable --item '{"Id":{"S":"123"},"Name":{"S":"Jane Doe"}}': Updates the Name attribute of an existing item.




aws dynamodb get-item

Explanation: Retrieves an item from a DynamoDB table by its primary key.
Options:
--table-name: Specifies the table.
--key: Specifies the primary key (e.g., {"Id":{"S":"123"}}).


Examples:
aws dynamodb get-item --table-name MyTable --key '{"Id":{"S":"123"}}': Retrieves an item with Id=123.
aws dynamodb get-item --table-name MyTable --key '{"Id":{"S":"123"},"SortKey":{"S":"A"}}': Retrieves an item with a composite key.





Intermediate Commands
These commands handle more complex tasks, such as managing security groups, bucket policies, IAM roles, and advanced DynamoDB operations.
1. EC2
Must-know Commands:

aws ec2 describe-security-groups

Explanation: Lists all security groups, including their IDs, names, and rules.
Options:
--group-ids: Filters by specific security group IDs.
--query: Extracts specific fields (e.g., SecurityGroups[*].GroupId).


Examples:
aws ec2 describe-security-groups --query 'SecurityGroups[*].{GroupId:GroupId,GroupName:GroupName}' --output table: Lists all security groups in a table.
aws ec2 describe-security-groups --group-ids sg-12345678 --query 'SecurityGroups[*].IpPermissions': Lists rules for a specific security group.




aws ec2 authorize-security-group-ingress

Explanation: Adds an inbound rule to a security group to allow specific traffic.
Options:
--group-id: Specifies the security group.
--protocol: Specifies the protocol (e.g., tcp, udp).
--port: Specifies the port or port range.
--cidr: Specifies the IP range (e.g., 0.0.0.0/0 for all IPs).


Examples:
aws ec2 authorize-security-group-ingress --group-id sg-12345678 --protocol tcp --port 80 --cidr 0.0.0.0/0: Allows HTTP traffic from anywhere.
aws ec2 authorize-security-group-ingress --group-id sg-12345678 --protocol tcp --port 22 --cidr 192.168.1.0/24: Allows SSH from a specific IP range.




aws ec2 create-volume

Explanation: Creates a new Elastic Block Store (EBS) volume for use with EC2 instances.
Options:
--availability-zone: Specifies the availability zone (e.g., us-west-2a).
--size: Specifies the volume size in GiB.
--volume-type: Specifies the volume type (e.g., gp2, io1).
--iops: Specifies IOPS for io1 volumes.


Examples:
aws ec2 create-volume --availability-zone us-west-2a --size 10 --volume-type gp2: Creates a 10 GiB general-purpose SSD volume.
aws ec2 create-volume --availability-zone us-west-2a --size 20 --volume-type io1 --iops 100: Creates a 20 GiB provisioned IOPS SSD volume with 100 IOPS.





2. S3
Must-know Commands:

aws s3 sync

Explanation: Synchronizes files between a local directory and an S3 bucket or between S3 locations, uploading or downloading only changed files.
Options:
--delete: Deletes files in the destination that are not in the source.
--exclude: Excludes specific files or patterns.
--include: Includes specific files or patterns.


Examples:
aws s3 sync /local/directory s3://my-bucket/path/: Uploads all files from a local directory to S3.
aws s3 sync s3://my-bucket/path/ /local/directory --delete: Downloads all files from S3 and deletes local files not in S3.




aws s3api put-bucket-policy

Explanation: Applies a bucket policy to control access to an S3 bucket, such as allowing public read access.
Options:
--bucket: Specifies the bucket name.
--policy: Specifies the policy document (inline JSON or file).


Examples:
aws s3api put-bucket-policy --bucket my-bucket --policy file://policy.json: Applies a policy from a local file.
aws s3api put-bucket-policy --bucket my-bucket --policy '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Principal":{"AWS":"*"},"Action":"s3:GetObject","Resource":"arn:aws:s3:::my-bucket/*"}]}': Applies an inline policy for public read access.





3. IAM
Must-know Commands:

aws iam create-role

Explanation: Creates an IAM role that trusted entities (e.g., EC2 instances, Lambda functions) can assume.
Options:
--role-name: Specifies the role name.
--assume-role-policy-document: Specifies the trust policy defining who can assume the role.


Examples:
aws iam create-role --role-name my-role --assume-role-policy-document file://trust-policy.json: Creates a role with a trust policy from a file.
aws iam create-role --role-name my-role --assume-role-policy-document '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Principal":{"Service":"ec2.amazonaws.com"},"Action":"sts:AssumeRole"}]}': Creates a role assumable by EC2 instances.




aws iam attach-role-policy

Explanation: Attaches a managed policy to an IAM role, granting specific permissions.
Options:
--role-name: Specifies the role.
--policy-arn: ARN of the policy to attach.


Examples:
aws iam attach-role-policy --role-name my-role --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess: Grants EC2 full access to the role.
aws iam attach-role-policy --role-name my-role --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess: Grants S3 read-only access to the role.




aws sts assume-role

Explanation: Temporarily assumes an IAM role, returning temporary security credentials for that role.
Options:
--role-arn: ARN of the role to assume.
--role-session-name: Name for the session (e.g., my-session).
--duration-seconds: Duration of the session (900 to 43200 seconds).


Examples:
aws sts assume-role --role-arn arn:aws:iam::123456789012:role/my-role --role-session-name my-session: Assumes a role and returns credentials.
aws sts assume-role --role-arn arn:aws:iam::123456789012:role/my-role --role-session-name my-session --duration-seconds 3600: Assumes a role for 1 hour.





4. DynamoDB
Must-know Commands:

aws dynamodb query

Explanation: Retrieves items from a DynamoDB table based on the primary key or secondary index.
Options:
--table-name: Specifies the table.
--key-condition-expression: Condition for the query (e.g., Id = :id).
--expression-attribute-values: Values for placeholders (e.g., {":id":{"S":"123"}}).


Examples:
aws dynamodb query --table-name MyTable --key-condition-expression "Id = :id" --expression-attribute-values '{":id":{"S":"123"}}': Queries for items with Id=123.
aws dynamodb query --table-name MyTable --key-condition-expression "Id = :id AND SortKey BETWEEN :start AND :end" --expression-attribute-values '{":id":{"S":"123"},":start":{"S":"A"},":end":{"S":"Z"}}': Queries with a composite key and range.




aws dynamodb scan

Explanation: Retrieves all items from a table, optionally filtered. Less efficient than query for large tables.
Options:
--table-name: Specifies the table.
--filter-expression: Filters results (e.g., Name = :name).
--expression-attribute-values: Values for placeholders.


Examples:
aws dynamodb scan --table-name MyTable: Scans the entire table.
aws dynamodb scan --table-name MyTable --filter-expression "Name = :name" --expression-attribute-values '{":name":{"S":"John Doe"}}': Scans for items where Name=John Doe.




aws dynamodb update-item

Explanation: Updates an existing item in a DynamoDB table, modifying specific attributes.
Options:
--table-name: Specifies the table.
--key: Specifies the item’s primary key.
--update-expression: Defines the update (e.g., set Name = :n).
--expression-attribute-values: Values for placeholders.


Examples:
aws dynamodb update-item --table-name MyTable --key '{"Id":{"S":"123"}}' --update-expression "set Name = :n" --expression-attribute-values '{":n":{"S":"Jane Doe"}}': Updates the Name attribute.
aws dynamodb update-item --table-name MyTable --key '{"Id":{"S":"123"}}' --update-expression "set Age = :a" --expression-attribute-values '{":a":{"N":"30"}}': Updates the Age attribute to 30.





5. Additional Services
VPC (Virtual Private Cloud)

aws ec2 describe-vpcs

Explanation: Lists all VPCs in your account, including their IDs, CIDR blocks, and tags.
Options:
--vpc-ids: Filters by specific VPC IDs.


Examples:
aws ec2 describe-vpcs: Lists all VPCs.
aws ec2 describe-vpcs --vpc-ids vpc-12345678: Lists details for a specific VPC.




aws ec2 create-vpc

Explanation: Creates a new VPC with a specified CIDR block.
Options:
--cidr-block: Specifies the IP range (e.g., 10.0.0.0/16).


Examples:
aws ec2 create-vpc --cidr-block 10.0.0.0/16: Creates a VPC with a /16 CIDR.
aws ec2 create-vpc --cidr-block 172.16.0.0/16: Creates a VPC with a different CIDR.





RDS (Relational Database Service)

aws rds describe-db-instances

Explanation: Lists all RDS database instances, including their IDs, engine types, and statuses.
Options:
--db-instance-identifier: Filters by a specific instance ID.


Examples:
aws rds describe-db-instances: Lists all RDS instances.
aws rds describe-db-instances --db-instance-identifier mydbinstance: Lists details for a specific instance.




aws rds create-db-instance

Explanation: Creates a new RDS database instance with specified parameters.
Options:
--db-instance-identifier: Specifies the instance name.
--db-instance-class: Specifies the instance type (e.g., db.t3.micro).
--engine: Specifies the database engine (e.g., mysql, postgres).
--allocated-storage: Specifies storage in GiB.
--master-username: Specifies the admin username.
--master-user-password: Specifies the admin password.


Examples:
aws rds create-db-instance --db-instance-identifier mydbinstance --db-instance-class db.t3.micro --engine mysql --allocated-storage 20 --master-username admin --master-user-password mypassword: Creates a MySQL instance.
aws rds create-db-instance --db-instance-identifier mypginstance --db-instance-class db.t3.micro --engine postgres --allocated-storage 20 --master-username admin --master-user-password mypassword: Creates a PostgreSQL instance.





Best Practices and Notes

Security: Avoid using root account credentials; create IAM users with least privilege. Use IAM roles for EC2 instances or other services to avoid hardcoding credentials.
CLI Version: Ensure you’re using AWS CLI version 2, as version 1 is deprecated for some features. Check with aws --version and update via the AWS CLI Installation Guide.
Help Command: Use aws <service> help (e.g., aws ec2 help) or aws <command> help (e.g., aws ec2 describe-instances help) for detailed documentation.
Output Formatting: Use --query and --output to customize responses for scripting or readability.
Error Handling: Check for common errors like AccessDenied (insufficient permissions) or InvalidParameterValue (incorrect parameters) and refer to the AWS CLI Troubleshooting Guide.
Automation: Combine commands in scripts for automation, but test thoroughly to avoid unintended changes.

Key Citations

AWS CLI Official Documentation: Comprehensive guide to AWS CLI usage and setup.
AWS CLI Command Reference: Detailed reference for all AWS CLI commands.
Blue Matador AWS CLI Cheatsheet: Quick reference for common AWS CLI commands.
[Abhay

