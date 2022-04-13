## Project Title - Deploy a high-availability web app using CloudFormation
This folder provides the starter code for the "Infrastructure as Code - Deploy a high-availability web app using CloudFormation" project. This folder contains the following files:

1. <strong>final-project-starter.yml</strong>:
Students have to write the CloudFormation code using this YAML template for building the cloud infrastructure, as required for the project. 

2. <strong>server-parameters.json</strong>:
Students may use a JSON file for increasing the generic nature of the YAML code. For example, the JSON file contains a "ParameterKey" as "EnvironmentName" and "ParameterValue" as "UdacityProject". 

In YAML code, the `${EnvironmentName}` would be substituted with `UdacityProject` accordingly.

## Scenario

* You have been tasked with provisioning the required infrastructure and deploying a dummy application, along with the necessary supporting software.

* This needs to be automated so that the infrastructure can be discarded as soon as the testing team finishes their tests and gathers their results.

* Optional - To add more challenge to the project, once the project is completed, you can try deploying sample website files located in a public S3 Bucket to the Apache Web Server running on an EC2 instance. Though, it is not the part of the project rubric.

## Server specs

* Create a Launch Configuration for your application servers in order to deploy 4 servers, 2 located in each of your private subnets. The launch configuration will be used by an auto-scaling group.

* 2 vCPUs, 4GB of RAM, Ubuntu 18, allocate at least 10GB of disk space.

## Security Groups and Roles

* Create an IAM Role that allows your instances to use the S3 Service.

* Udagram communicates on the default HTTP Port: 80, so your servers will need this inbound port open since you will use it with the Load Balancer and the Load Balancer Health Check. As for outbound, the servers will need unrestricted internet access to be able to download and update their software.

* The load balancer allow all public traffic (0.0.0.0/0) on port 80 inbound. Outbound, it will only be using port 80 to reach the internal servers.

* The application needs to be deployed into private subnets with a Load Balancer located in a public subnet.

* One of the output exports of the CloudFormation script should be the public URL of the LoadBalancer. Bonus points if you add http:// in front of the load balancer DNS Name in the output, for convenience.

## Other Considerations

1. Deploy your servers with an SSH Key into Public subnets while you are creating the script. This helps with troubleshooting. Once done, move them to your private subnets and remove the SSH Key from your Launch Configuration.

2. It also helps to test directly, without the load balancer. Once you are confident that your server is behaving correctly, increase the instance count and add the load balancer to your script.

3. While your instances are in public subnets, you'll also need the SSH port open (port 22) for your access, in case you need to troubleshoot your instances.

4. Log information for UserData scripts is located in this file: cloud-init-output.log under the folder: /var/log.

5. Destroy the entire infrastructure and build it back up without any manual steps required, other than running the CloudFormation script.

6. The UserData script install all the required dependencies. This process takes several minutes to complete. Also, the application takes a few seconds to load.

7. If you want to go the extra mile, set up a bastion host (jump box) to allow you to SSH into your private subnet servers. This bastion host would be on a Public Subnet with port 22 open only to your home IP address, and it would need to have the private key that you use to access the other servers.

8. Remember to delete your CloudFormation stack when you're done to avoid recurring charges!

## PROJECT SPECIFICATION
### The Basics

| CRITERIA      | MEETS SPECIFICATIONS |
| ----------- | ----------- |
| Parameters  | The more the better, but an exaggerated number of parameters can be messy ( say, 10 or more ). 1 or 0 is definitely lacking.    |
| Resources   | This is the mandatory section of the script, we are looking for a LoadBalancer, Launch Configuration, AutoScaling group a health check, security groups and a Listener and Target Group.  |
| Outputs     | This is optional, but it would be nice to have a URL here with the Load Balancer DNS Name and “http” in front of it .            |
| Working Test| If the student provides a URL to verify his work is running properly, it will be a page that says “it works! Udagram, Udacity”    |

### Load Balancer

| CRITERIA      | MEETS SPECIFICATIONS |
| ----------- | ----------- |
| Target Group              | The auto-scaling group needs to have a property that associates it with a target group. The Load Balancer will have a Listener rule associated with the same target group        |
| Health Check and Listener | Port 80 should be used in Security groups, health checks and listeners associated with the load balancer            |

### Auto-Scaling

| CRITERIA      | MEETS SPECIFICATIONS |
| -----------   | ----------- |
| Subnets       | Students should be using PRIV-NET ( private subnets ) for their auto-scaling instances        |
| Machine Specs | The machine should have 10 GB or more of disk and should be a t3.small or better.        |
| SSH Key       | There shouldn’t be a ‘keyname’ property in the launch config        |

### Bonus

| CRITERIA      | MEETS SPECIFICATIONS |
| ----------- | ----------- |
| Output      | Any values in the output section are a bonus         |
| Bastion Host| Any resource of type AWS::EC2::Instance, optional, but nice to have.        |

## Suggestions to Make Your Project Stand Out!
* Students can deploy Windows Servers instead of Linux and use PowerShell scripts to showcase their Windows management skills.
* Students can use AWS Parameter Store to save sensitive data, such as credentials to showcase their attention to security.
* Students can use CloudWatch Alarms and CloudWatch custom metrics to showcase their performance and monitoring skills.


```sh
aws cloudformation create-stack  --stack-name finalproject1 --region us-east-1 --template-body file://network.yml --parameters file://network-parameters.json --capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM"
```

```sh
aws cloudformation create-stack  --stack-name finalproject2 --region us-east-1 --template-body file://server.yml --parameters file://server-parameters.json --capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM"
```