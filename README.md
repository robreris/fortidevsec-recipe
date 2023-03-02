# AWSWorkshop.io base workshop 

This is a base workshop. Clone and start from this repo to create your workshop.

## Versions
 * 1.0
    * Initial Release:
    Overhauled to add prescriptive guidance. Improved readability and ease of use by making the base more templatized.


## Description

 This is the base repo for building workshops with AWS. It utilizes the Hugo framework which involves simple mark down and HTML elements.

 In this workshop you are going to learn how to plan, build, and launch an AWS workshop.

## What is a workshop?

 An AWS workshop is a tool used to educate users and end customers on how to leverage partner solutions on their AWS workload. What better way to learn than to let customers get hands on with building or instrumenting products or services in an actual AWS environment? A workshop format is used because it scales well, meaning you can deliver the content and message whenever and wherever the customer happens to be: whether the customer is at work, home, or at an AWS event, they can get hands on and learn about building products and solutions.

## High level Planning

 While creating your workshop you will want to think about what you want to accomplish and how you want to educate users about your product. Depending on your needs you will either build a new workshop or use an existing one and modify it as you see fit.

 You will want to create a high-level plan for your workshop and determine the problem you are looking to solve. Identifying key concepts that you want the customers to learn about is also ideal. Then outlining what components and AWS services that you are going to utilize is also a key step. This will play a big role in creating the workflow that your customers will be following along with during the workshop. The workflow should be presented as a story and have an introduction, an educational body, and a conclusion that ties all the pieces together. A cleanup section will follow suit so that the customers can make sure their environments will not be charged after they finish the workshop. Determining what kind of event this workshop will be presented at is also rather important as it will help you have a way to capture leads which should be the end goal in mind. (More details are covered in the workshop itself)

## Types of Events
 
 Identifying whether your workshop will be a self-paced workshop or an AWS hosted event will be crucial in your planning as well. If it’s a self-paced workshop then having highly contextualized sections will be very important as there won’t be anyone there to answer questions. Making sure that the workshop itself has all relevant information or that clear references have been outlined for any documentation that will be needed to successfully complete the workshop are crucial to the workshops overall success.

## Build

 In this section you will be setting up your workflow, edit and build, test your environments, and publish the workshop if everything is prescriptive enough. Follow along to learn how to complete all of these tasks. 

#### Deploy using the Dockerfile
 
 After cloning the repo, from the base directory, run the following commands to deploy the app locally. 
 
 ```
 docker build -t app-workshop .
 docker run -p 1313:1313 app-workshop:latest
 ```
Open a browser and navigate to http://localhost:1313 to view the app.

##### Using the CLI
 
 Open a terminal and ensure the AWS_DEFAULT_REGION and AWS_PROFILE parameters are set.
 
 ```
 export AWS_DEFAULT_REGION=<your region>
 export AWS_PROFILE=<your profile as configured in your local AWS credentials file>
 ```
 
 You will need to have an ECR repository set up before proceeding. You can create one via the following command with the CLI:
 
 ```
 aws ecr create-repository --repository-name <ECR Repo Name> --image-scanning-configuration scanOnPush=true
 ```

 Be sure to note down the repositoryUri displayed in the output.
 
 You'll need to build and push the app image to the repository using the following commands. Be sure to substitute in appropriate values for your setup (such as region, etc.)
 
 ```
 aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <Your AWS acct number>.dkr.ecr.<region>.amazonaws.com
 docker build -t app-workshop .
 docker tag app-workshop:latest <reopsitoryUri>:latest
 docker push <repositoryUri>:latest
 ```
 
 Retrieve the ECS CloudFormation template and parameter file from the utility repo:

 ```
 wget https://raw.githubusercontent.com/rob-40net-test/cft-utility-templates/main/ecs-app-params.json
 wget https://raw.githubusercontent.com/rob-40net-test/cft-utility-templates/main/ecs-app-template.yml
 ``` 

 Open the ecs-app-params.json file and fill in the parameters as in this example: 

 ```
 [
        {
                "ParameterKey": "ECRRepo",
                "ParameterValue": "<repositoryUri from output above>"
        },
        {
                "ParameterKey": "ECRRepoName",
                "ParameterValue": "<name of repository or repositoryName from output above>"
        },
   
        {
                "ParameterKey": "KeyPair",
                "ParameterValue": "<the name a key pair from your account>"
        },
        {
                "ParameterKey": "AppVPC",
                "ParameterValue": "vpc-123abc456"
        },
        {
                "ParameterKey": "AppSubnet",
                "ParameterValue": "subnet-789efg101112h"
        },
        {
                "ParameterKey": "AllowedCidr",
                "ParameterValue": "0.0.0.0/0"
        }
]
 ```

 Be sure to choose a VPC with an Internet Gateway and a subnet with a route table containing an entry with a target set to 0.0.0.0/0 and destination the Internet Gateway in order for the app to be accessible from the public internet.

 Deploy the stack with the following command:

 ```
 aws cloudformation create-stack --stack-name <enter a name for your stack here> \
    --template-body file://./ecs-app-template.yml --parameters file://./ecs-app-params.json \ 
	--capabilities CAPABILITY_NAMED_IAM
 ```
 
The public IPv4 address of the instance where the container is deployed can be found by navigating to the EC2 Instances console and selecting the instance labeled "ECS Instance - <your stack name>". The app can be viewed in a browser by accessing: 
 
```
 http://<ec2 instance public IPv4>:1313
``` 

 To make changes, you can just rebuild the image and push again:
 
 ```
 docker build -t app-workshop .
 docker tag app-workshop:latest <repositoryUri>:latest
 docker push <repositoryUri>:latest
 ```
 
 When finished testing, you can delete the stack:
 
 ```
 aws cloudformation delete-stack --stack-name <your stack name>
 ```

## Launch

 With your workshop now being published, you can now identify some key got to market activities. 

## FAQ

 Commonly asked questions along with tools, tips, and samples that might be relevant to your workshop. Modify this FAQ section as you best see fit for your specific workshop and your customer base. 

## Authors

Contributors names and contact info

* James Bland (@jamesbland123)
* Parker Perry (@parkerperry)  
* James Spencer (@folrig)
* Eugene Mu (@eugenemu)

 ## License

This project is licensed. See the LICENSE.md file for details

## Acknowledgments

* Markdown cheat sheet (https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* Learn theme markdown (https://learn.netlify.app/en/cont/markdown/)
* Menu extras and shortcuts (https://learn.netlify.app/en/cont/menushortcuts/) 
* Using Font Awesome Emoji's to help your page pop (https://learn.netlify.app/en/cont/icons/)
