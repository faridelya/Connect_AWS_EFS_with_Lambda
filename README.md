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
![ALT text}(https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/pic/3%20lambda.png)


