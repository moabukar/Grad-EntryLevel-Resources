# AWS Tests

## Task 1

Setup an EC2 Instance with an IAM Role so that it can read, write and delete from your S3 Bucket (using the aws-cli). 

If you get that working consider using S3 Events to send a text message or email to yourself when a file is added to your S3 bucket from the instance:


<details>
<summary>Task 1 Solution 
(Check once you finish the task): </summary>
<br>

1 Spin up an EC2 Instance on AWS (make sure you have your SSH Key, you'll need it)

2 SSH Into the Instance

```sh
ssh -i "<Your-pem-file.pem" ec2-user@ec2-IP.region.compute.amazonaws.com
```

3 Create an S3 bucket



</details>