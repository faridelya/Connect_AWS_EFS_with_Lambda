Setting lambda on pycharm locally [click](https://www.jetbrains.com/pycharm/guide/tutorials/intro-aws/setup/) here
# Connect AWS EFS with AWS Lambda function.
This repository will show you How we can utilize use AWS EFS drive with the lambda function when we face storage constrain. 

### Core steps that we will cover throughout this are as under.
1. Create a lambda function.
2. Create a File system ( EFS ) and configure **network** and **access point** options.
3. Create an Aws EC2 instance that will mount to EFS  and you can put packages or libraries.
4. Mount EFS with Lambda function just and click **Configuration** and set up these two options (1) **VPC** (2) **File system**.


#### step 1:
create lambda Function by filling in options and selecting the Python version and clicking create button.
![ALT text](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/pic/3%20lambda.png)

- Set role in lambda permission ```AWSLambdaBasicExecutionRole``` , ```AWSLambdaVPCAccessExecutionRole``` , ```AmazonElasticFileSystemClientFullAccess```

#### step 2:
create an EFS  drive so you can put your application packages on this drive and mount to the ec2 instance for  putting or installing packages in the mount efs path.
![ALT text](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/pic/2%20efs%20create.png)

- create an access point in efs:
- you will set these configuration in access point and also mount folder name.
  ![ALT text] (https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/pic/access-point.png)

- Create Network setting in EFS:
- you should use those security group that you have attach with Ec2 instance which has the following ports open for inbound :
- 	
IPv4	HTTP	TCP	  80	   0.0.0.0/0	
IPv4	HTTPS	TCP	  443	   0.0.0.0/0	
IPv4	SSH	  TCP	  22	   0.0.0.0/0	
IPv4	NFS	  TCP	  2049	 0.0.0.0/0
![ALT text](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/pic/2%20efs%20network.png)
