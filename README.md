# Go-Ceries Serverless React Application
A Prototype E-Commerce Provision Service 

## This application is separated into two projects

### Backend Serverless Application
The backend of the application is built using the servless framework connected to AWS
This will deploy the following resources:
* AWS Lambda with the require functions (on Node Environment)
* AWS API Gateway to handle all API calls
* AWS DynamoDB to store the data
* AWS Cognito to manage user authentication
* AWS S3 Bucket to store any necessary files

### Frontend React Application
A single page application built using:
* React
* Material UI
* Mobx


### Requirements

- [Install the Serverless Framework](https://serverless.com/framework/docs/providers/aws/guide/installation/)
- [Configure your AWS CLI](https://serverless.com/framework/docs/providers/aws/guide/credentials/)
- [Install ECS-CLI](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_installation.html)
- [Install Docker](https://docs.docker.com/install/)
- [Install Docker-Compose](https://docs.docker.com/compose/install/)
- [Install Node](https://nodejs.org/en/)

### Installation for Setting up Go-Ceries Backend Application

Clone go-ceries-server repository

``` bash
$ git clone https://github.com/HaifengMei/go-ceries-server
```
Go to root of go-ceries-server repository 

``` bash
$ cd go-ceries-server
```
Install the Node.js packages

``` bash
$ npm install
```

Configure your aws accessKeyId and accessKeyId. Set region as us-east-1 
``` bash
$ aws configure
```

Deploy the go-ceries-server

``` bash
$ serverless deploy -v
```

Take note of the output VALUES when the command finish ran:
* AttachmentsBucketName
* ServiceEndpoint
* UserPoolId
* UserPoolClientId
* IdentityPoolId

![Servless Output](https://github.com/HaifengMei/go-ceries-server/blob/master/Screenshots/Serverless%20Deploy%20Outputspng.png?raw=true)

### Log on to AWS Console and go to CloudFormation to verify your stack deployed
![CloudFormation Serverless Stack](https://github.com/HaifengMei/go-ceries-server/blob/master/Screenshots/Deploy%20Serverless%20stack.PNG?raw=true)

### Instructions for Setting up Go-Ceries Front-End Application

Clone go-ceries-server repository

``` bash
$ git clone https://github.com/HaifengMei/go-ceries-cloud
```
Go to root of go-ceries-server repository 

``` bash
$ cd go-ceries-cloud
```

## Update aws-config.js file
1. This is found in go-ceries-cloud/src/config/aws-config

    Change the following KEY VALUES (Not the key):

    In 'const dev': 
    
    * Change the 'BUCKET' value to the 'AttachmentsBucketName' value obtained in the outputs.
    * Change the 'URL' value to the 'ServiceEndpoint' value obtained in the outputs.
    * Change the 'USER_POOL_ID' value to the 'UserPoolId' value obtained in the outputs.
    * Change the 'APP_CLIENT_ID' value to the 'UserPoolClientId' value obtained in the outputs.
    * Change the 'IDENTITY_POOL_ID' value to the 'IdentityPoolId' value obtained in the outputs.
        
    Repeat this process for the same values in 'const prod'.
        
2. Go onto your AWS Console and then go to EC2 Dashboard
    Here create a KeyPair Value and save it in your root directory of the Go-Ceries application folder.

3. Go to the aws-compose.yml file in the root directory of the Go-Ceries application and make the following changes.
    In web:
        Change the 'image' value to '[Your Docker Hub Username]/go-ceries-cloud_app'

4. Make similar changes to the 'commands.sh' file in the root directory of the Go-Creies Application
    In line 1:
    ``` bash
    sudo docker build -t [Your Docker Hub Username]/go-ceries-cloud_app
    ```
    
    In line 2
    ``` bash
    sudo docker push [Your Docker Hub Username]/go-ceries-cloud_app
    ```
    
    In line 4
    ``` bash
    ecs-cli up --keypair [Your KeyPair File Name] --capability-iam --size 2 --instance-type t2.micro
    ```
    
    Confirm the created attack in your AWS CloudFormation Dashboard
    ![CloudFormation Serverless Stack](https://github.com/HaifengMei/go-ceries-server/blob/master/Screenshots/Frontend%20App%20Stack.PNG?raw=true)
        
4. Afterwards either run the commands.sh file or copy and paste the commands into your command line and run them.

5. Find the IP of your deployed Front-end Application
    ``` bash
    ecs-cli ps
    ```
    http://[Your IP Address] , (eg. http://54.157.255.187)
    Enter it into a web browser of your choice.
    

