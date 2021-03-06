# AWS Tests

## Task 1 - EC2, S3 buckets and SNS Topics

Setup an EC2 Instance with an IAM Role so that it can read, write and delete from your S3 Bucket (using the aws-cli). 

If you get that working consider using S3 Events to send a text message or email to yourself when a file is added to your S3 bucket from the instance.

The solutions are in the dropdowns below, only check after you have attempted the tasks.
l
Check the Prerequisites dropdown to ensure you have AWS CLI installed and configured on your system - the below solution if for Mac users.

<details>
<summary> Prerequisites </summary>
<br>

1 Make sure you have at least Python 2 Installed. Check to see which version of Python you have by running:

```sh
$ python --version 
```

2 Install the AWS CLI by running the command: 

```sh
$ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
```

3 Unzip the package from (2):

```sh
$ unzip awscli-bundle.zip
```

4 Run the installation:

```sh
$ sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
```

5 Follow IAM on your AWS console, and generate your Secret Access Key and put it somewhere safe

6 Change directory on your terminal to where you saved your file:

```sh
$ cd /path/to/your/access-key.csv
```

7 Run the AWS configure command:

```sh
$ aws configure
AWS Access Key ID [None]: <Fill this in>
...
```
Fill the details from your CSV file, put in the region and choose JSON as your default output format

8 Protect the config file, which will be located in the ~/.aws/ directory

```sh
$ chmod 600 ~/.aws/config
```

9 That's it - you're ready to use AWS on your console. Test it out by running the following command:

```sh
$ aws s3 ls 
```

And if you have an S3 buckets, they will be listed as per the 'ls' command

</details>




<details>
<summary>Task 1 Solution - Part 1
(Check once you finish the task): </summary>
<br>


1 Spin up an EC2 Instance on AWS, and when you reach the IAM roles, give it the full access S3 IAM Role

2 When you press launch, create a new key-pair and download it. When downloaded move it to your chosen directory:

```sh
$ mv ~/Downloads/<your-key-pair-file>.pem ~/path/to/your/chosen/directory
```

The .pem file is now in your chosen directory

2 SSH Into the Instance - if not in your chosen directory, change it to that

```sh
$ ssh -i "<Your-pem-file.pem" ec2-user@ec2-IP.region.compute.amazonaws.com
```
If you've done everything right so far, you are now in your EC2 server via your terminal

3 Create an S3 bucket, with default options from the console

4 Go back to your EC2 server via the terminal and test if it has read, write and delete permissions with your bucket

```sh
$ echo "Hello World" | aws s3 cp - s3://yourbucketname/test.txt
```

Go to the S3 bucket via the console to see if your txt file is there, and see what it contains. This shows it has write files.

```sh
$ aws s3 cp s3://yourbucketname/test.txt ./
```

This will copy your S3 object to your current directory in your EC2 server. List contents to see if it's there (ls)

```sh
$ aws s3 rm s3://yourbucketname/test.txt
```

This will delete the file from the S3 bucket. You can also delete from the EC2 by running:

```
$ rm test.txt
```

</details>


<details>
<summary>Task 1 Solution - Part 2:  </summary>
<br>

For this part you will need to use S3 events, SNS to receive emails and EC2 instance to add or remove objects from an S3 bucket.

1 On your S3 bucket, go to Event Notifications - You will find this in the properties tab on the S3 bucket

2 Name it and select which events it should trigger on - for the case above, this is on a put.

3 When you reach the destination, you will notice that you need to configure an SNS topic first

4 Go to the SNS service on AWS, and create an SNS topic (Standard, NOT FIFO)

5 Also, create an IAM role for the SNS topic to allow the SNS your bucket ARN as required

6 On the SNS Access Policy, ensure you get rid of any conditions present, and replace it with 

```sh
"Condition": {
    "ArnLike": {
        "AWS:SourceArn": "your-chosen-bucket-ARN"
}
```
7 Make the SNS Topic and then proceed to selecting this Topic on your S3 bucket Event Notifications

8 Now for the email bit, create a Subscription for your newly created SNS topic and select email

9 Go back to your EC2 instance (via terminal), and run the commands from Part 1: 

```sh
$ echo "Hello World" | aws s3 cp - s3://yourbucketname/test.txt
```

You will receive an email telling you an object was added to your S3 bucket - proving that you have successfully configured S3 notifications, SNS topics in addition to roles and permissions. Well done!


</details>
