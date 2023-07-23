# Deploy Custom Container on Lambda function 

- As we known lambda function has limtion when it comes to large size of application and also when we want to install system packages for our Application. Lambda allow you 250 MB of total size of unzip files of Application and we also allow to create 5 layer each of them provide 50MB of size and a total of 5 * 50 = 250 MB in total for 5 layer.
- So to overcome this problem we can build Docker container max size of 10 GB which is allowed in lambda function. 
- AWS also provide its own base images for creation of Docker images but i will use Debian flavour of linux which we use normally called  ( Ubuntu ). so we can install apt packages that are dependences for our Application.

### Following steps are inculded in creation of Custom container for Lambda.

1. #####  Create Iam User and add Permission for that USER Group.
2. ##### Configure AWS CLI locally by providing Acces key and Secret key.
3. ##### Create Dockerfile and lambda function locally.
4. ##### Build Docker image and Test.
5. ##### Push Docker Image to AWS ECR.
4. ##### Attach to Lambda Function.

### Step 1: Create Iam User
- Create Iam User and add Permission for that USER Group.
- Goto IAM User,  click ADD USER
  ![1](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Iam/1%20User.png)
- Create one User
  ![write user name ](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Iam/2%20User.png)
- ADD this User to group which has good amount of permissions if not, create one for yourself
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Iam/3%20User.png)
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Iam/4%20User.png)
- Click create and User will be added.
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Iam/5%20User%20created.png)
- Check User Group Permissions and the new created user is already added to this group.
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Iam/6%20User%20group%20permission.png)
- Goto ```Security``` option and create ```Access key```.
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Iam/7%20User%20Access%20key%201.png)
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Iam/8%20User%20access%20key%20form.png)
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Iam/9%20User%20add%20tag.png)
- Access and Security key created successfully copy and save in txt or csv file, it will use in aws configure step.
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Iam/10%20User%20key%20created.png)
  
  


### Step 2: Configure AWS CLI
- You must have install AWS cli on laptop and Configure AWC CLI Locally , open terminal and run ```aws configure```
- add Access key and enter
- add Secret key and enter and other two option remain empty just enter enter. 
  ![aws configuration](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/aws%20config/1%20AWS%20configure.png)

### Step 3: Create Dockerfile
- Before Creating Dockerfile and developing lambda Function INSTALL [Docker](https://docs.docker.com/engine/install/ubuntu/) and also apply [Post](https://docs.docker.com/engine/install/linux-postinstall/) requirement steps.

- Create ```Dockerfile``` inside folder where you have placed ```app.py``` module which contain lambda function.
- **```app.py```**
    ```
    import boto3
    import sys
    
    
    def handler(event, context):
        s3 = boto3.client('s3')
        bucket_name = 'your-bucket-name'  
        key = event['file']['filename']
        encoded_string = event['file']['content'] # base64encode file
        text_file_key = 'files/base64.txt'  # Specify the desired S3 key for the text file
        s3.put_object(Body=clean_text, Bucket=bucket_name, Key=text_file_key) # saved in s3 
        
        return 'Hello from AWS Lambda using Python' + sys.version + '!'
    ```

- **```Dockerfile```**
    ```
    # this is not aws base image this is outsider or custom ,so we can install apt packages on this image
    FROM python:3.8-slim-buster  
    
    # you can install packages here now its optional just to show you
    RUN apt-get update && apt-get install -y \
        python3-dev 
    
    # Set the working directory in the Docker image
    WORKDIR /App
    
    # Copy the requirements.txt file to the working directory
    COPY requirements.txt .
    
    # Install Python dependencies
    RUN pip install awslambdaric  # aws run time interface  compulsory
    RUN pip install --no-cache-dir -r requirements.txt
    
    # Copy the code file to the working directory
    COPY app.py .
    
    ENTRYPOINT [ "/usr/local/bin/python", "-m", "awslambdaric" ]
    
    # Pass the name of the function handler as an argument to the runtime
    CMD [ "app.handler" ]
    ```
- **```requirements.txt```**
- you can any package name here for example.
    ```
    python-magic
    ```
### Step 4:  [Build Docker image and Test and Push ECR](https://docs.aws.amazon.com/lambda/latest/dg/python-image.html#python-image-clients:~:text=Deploying%20the%20image-,Using%20an%20alternative%20base%20image%20with%20the%20runtime%20interface%20client,-If%20you%20use).
- Open terminal inside folder where Dockerfie and app.py exist.
- Run for build image.Here docker-image is name and test is tag you can change both
    ```
    docker build -t docker-image:test .       
    ```
- **Testing Container locally**
- To locally test an image built from a base image for custom runtimes or an alternative base image, you must use the runtime interface emulator. You can build the emulator into your image or install it on your local machine.
- To install and run the runtime interface emulator on your local machine
1. **From your project directory, run the following command to download the runtime interface emulator (x86-64 architecture)**
-   -- from GitHub and install it on your local machine.
    ```
    mkdir -p ~/.aws-lambda-rie && \
        curl -Lo ~/.aws-lambda-rie/aws-lambda-rie https://github.com/aws/aws-lambda-runtime-interface-emulator/releases/latest/download/aws-lambda-rie && \
        chmod +x ~/.aws-lambda-rie/aws-lambda-rie
    ```
2. **Start the Docker image with the docker run command. Note the following:**
- ```docker-image``` is the image name and ```test``` is the tag.

- ```/usr/local/bin/python -m awslambdaric lambda_function.handler``` is the ```ENTRYPOINT``` followed by the ```CMD``` from your Dockerfile.
    ```
    docker run -d -v ~/.aws-lambda-rie:/aws-lambda -p 9000:8080 \
    --entrypoint /aws-lambda/aws-lambda-rie \
    docker-image:test \
        /usr/local/bin/python -m awslambdaric lambda_function.handler
    ```
- This command runs the image as a container and creates a local endpoint at ```localhost:9000/2015-03-31/functions/function/invocations```.

3. **From a new terminal window, post an event to the following endpoint using a curl command:**
    ```
    curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'
    ```
- This command invokes the function with an empty event and returns a response. Some functions might require a JSON payload. Example:
    ```
    curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{"payload":"hello world!"}'
    ```
4. **Get the container ID.**
    ```
    docker ps
    ```
- Use the docker kill command to stop the container. In this command, replace 3766c4ab331c with the container ID from the previous step.
    ```
    docker kill 3766c4ab331c
    ```
## Step 5: Push Docker Image to AWS ECR.
- first create ECR Repositroy
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/ECR/1%20Push%20ECR.png)
- Open repo and inside click ``` View Push Commands```.
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/ECR/2%20inside%20ecr%20.png)
- Check Docker Images are added.
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/ECR/3%20ecr%20push%20command.png) 

    
    
## step 6:  Attach to Lambda Function:
- Create Lambda function and chose docker container and select ECR repo and select image.
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Lambda/1%20create%20lambda%20image.png)
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Lambda/2%20created%20lambda.png)
- Click on role for adding new role for lambda
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Lambda/3%20lambda%20configure%20for%20permission.png)
- You must have these permission for lambda if you use s3 bucket.
  ![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/create_custom_container_for_lambda/images/Lambda/4%20add%20role%20for%20lambda.png)

- You can attach ```APIGATEWAY``` to this lambda function and also enble cors for apigateway
    

