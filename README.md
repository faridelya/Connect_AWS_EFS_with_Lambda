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

  ##### Create an access point in efs:
- you will set these configurations in the access point and also mount the folder name.
![ALT text] (https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/pic/access-point.png)

  ##### Create Network setting in EFS:
- you should use those security group that you have attach with Ec2 instance which has the following ports open for inbound :
	
- IPv4	HTTP	TCP	  80	   0.0.0.0/0	
- IPv4	HTTPS	TCP	  443	   0.0.0.0/0	
- IPv4	SSH	  TCP	  22	   0.0.0.0/0	
- IPv4	NFS	  TCP	  2049	 0.0.0.0/0
- 
![ALT text](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/pic/2%20efs%20network.png)

#### step 3 
**Connect with EC2 instance:**
When you are going to connect efs with ec2 instance first connect to your ec2 instance and install aws efs-utils 
 - To build and install [amazon-efs-utils](https://docs.aws.amazon.com/efs/latest/ug/installing-amazon-efs-utils.html) as a Debian package for Ubuntu and Debian
 - To clone amazon-efs-utils from GitHub
 - Connect to the EC2 instance using Secure Shell (SSH), and log in with the appropriate user name. For more information, see Connecting to Your Linux Instance Using SSH in the Amazon EC2 User Guide for Linux Instances.
 - (Optional) Apply updates before installing the package with the following command:
```
sudo apt-get update
```
- Install updates as needed.

- Install git and binutils, using the following command. binutils is required for building DEB packages,
```
sudo apt-get -y install git binutils
```
- Clone amazon-efs-utils from GitHub using the following command.
```
git clone https://github.com/aws/efs-utils
```
- To build and install the amazon-efs-utils DEB package
- Navigate to the directory that contains the amazon-efs-utils package.
```
cd /path/efs-utilswwwwwssw
```
- Build amazon-efs-utils using the following command:
```
./build-deb.sh
```
- Install the package with the following command.
```
sudo apt-get -y install ./build/amazon-efs-utils*deb
```

- **Now go to EFS and copy mount command and paste in ec2 instance but make directory for mount pout and you will place that folder name at the end of the command.**

- now move to the mount point directory and create one folder with the same name you added in access point of efs  and inside that folder install any package.
-  if you found a permission error use these commands but get User and Group Id from the access point of EFS.

```
sudo chmod USERID:GROUPID /path/to/mountpoint
```
```
sudo chmod -R 777 mountpoint
```
- Now you can install anything in mount point of EFS

#### setp 4
**Mount EFS with Lambda function**
- goto lambda - configuration --> VPC
- you will set the same security group and the region that you have used in EFS network setting  fill option and apply
![ALT text](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/pic/1vpc.png)
