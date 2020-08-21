# Deploy a high-availability web app using CloudFormation, Project Starter Code

The objective of this project is to deploy web servers for a highly available web app using CloudFormation.

<!--more-->

[//]: # (Image References)

[image1]: ./images/Diagrama_AWS.jpeg "AWS Diagram"
[image2]: ./images/Bucket1.jpg "Development Bucket"
[image3]: ./images/Bucket2.jpg "Development Bucket"
[image4]: ./images/Bucket3.jpg "Development Bucket"
[image5]: ./images/Bucket4.jpg "Development Bucket"
[image6]: ./images/Bucket5.jpg "Development Bucket"
[image7]: ./images/Bucket6.jpg "Development Bucket"
[image8]: ./images/Bucket7.jpg "Development Bucket"
[image9]: ./images/Bucket8.jpg "Development Bucket"
[image10]: ./images/KeyPairs.jpg "Key Pairs"
[image11]: ./images/01Subida.jpg "Deployment"
[image12]: ./images/02Subida.jpg "Deployment"
[image13]: ./images/03Subida.jpg "Deployment"
[image14]: ./images/04Subida.jpg "Deployment"
[image15]: ./images/05Subida.jpg "Deployment"
[image16]: ./images/06Subida.jpg "Deployment"
[image17]: ./images/07Subida.jpg "Deployment"
[image18]: ./images/08Subida.jpg "Deployment"
[image19]: ./images/09Subida.jpg "Deployment"
[image20]: ./images/10Subida.jpg "Deployment"
[image21]: ./images/11Subida.jpg "Deployment"
[image22]: ./images/12Subida.jpg "Deployment"
[image23]: ./images/13Subida.jpg "Deployment"
[image24]: ./images/14Subida.jpg "Deployment"
[image25]: ./images/15Subida.jpg "Deployment"
[image26]: ./images/16Subida.jpg "Deployment"
[image27]: ./images/17Subida.jpg "Deployment"
[image28]: ./images/18Subida.jpg "Deployment"
[image29]: ./images/19Subida.jpg "Deployment"
[image30]: ./images/20Subida.jpg "Deployment"
[image31]: ./images/21Subida.jpg "Deployment"
[image32]: ./images/website.jpg "Website"

---


#### How to run the program with your own code

For the execution of your own code, in the Visual Studio Code application, we open a terminal with powershell and run our CloudFormation script.

##### For Windows systems, we have the following scripts:

To create the stack from the project.
```bash
  .\Create_Stack.bat UdagramApp structure_network_server.yml structure_network_server.json
```

To update the stack from the project.
```bash
  .\Update_Stack.bat UdagramApp structure_network_server.yml structure_network_server.json
```

To delete the stack from the project.
```bash
  .\Delete_Stack.bat UdagramApp
```

##### For Linux systems, we have the following scripts:

To create the stack from the project.
```bash
  ./Create_Stack.sh UdagramApp structure_network_server.yml structure_network_server.json
```

To update the stack from the project.
```bash
  ./Update_Stack.sh UdagramApp structure_network_server.yml structure_network_server.json
```

To delete the stack from the project.
```bash
  ./Delete_Stack.sh UdagramApp
```


---

The summary of the files and folders within repo is provided in the table below:

| File/Folder              | Definition                                                                                                   |
| :----------------------- | :----------------------------------------------------------------------------------------------------------- |
| images/*                 | Folder containing the images of the project.                                                                 |
| scripts/*                | Folder containing the project execution scripts.                                                             |
|                          |                                                                                                              |
| jumboxKP.pem             | File containing the key pair of the jump box machine.                                                        |
| structure_network_server.json | File containing the project parameters.                                                                 |
| structure_network_server.yml | Template containing all network coding, servers and their configurations.                                |
| udacity.zip              | Compressed file containing the website downloaded by the development team.                                   |
| UdacityMadridKP.pem      | File containing the key pair of the machines in the private networks.                                        |
|                          |                                                                                                              |
| README.md                | Contains the project documentation.                                                                          |
| README.pdf               | Contains the project documentation in PDF format.                                                            |


---

**Steps to complete the project:**

#### Problem.

1. Your company is creating an Instagram clone called Udagram. Developers pushed the latest version of their code in a zip file located in a public S3 Bucket.
2. You have been tasked with deploying the application, along with the necessary supporting software into its matching infrastructure.
3. This needs to be done in an automated fashion so that the infrastructure can be discarded as soon as the testing team finishes their tests and gathers their results.


## [Rubric Points](https://review.udacity.com/#!/rubrics/2556/view)
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
## Scenario.

In this project, we'll deploy web servers for a highly available web app using CloudFormation. We'll write the code that creates and deploys the infrastructure and application for an Instagram-like app from the ground up. We'll begin with deploying the networking components, followed by servers, security roles and software.


## AWS Diagram.

The diagram we have designed and will use for the Udagram application is as follows:

![alt text][image1]


#### Version of software tools used for this project.

* Visual Studio Code/1.48.0
* AWS CLI/1.18.120
* Python/2.7.17
* Windows/10
* Botocore/1.17.43


## Project deployment.

#### Create S3 Bucket.

Navigate to the “AWS Management Console” page, type “S3” in the “Find Services” box and then select “S3”.

![alt text][image2]

The Amazon S3 dashboard displays. Click “Create bucket”.

![alt text][image3]

Enter a “Bucket name” and click “Next”. Note: Bucket names must be globally unique.

![alt text][image4]

Click “Next” again to skip over “Step 2: Configure Options”.

On “Step 3: Set Permissions”, uncheck “Block all public access”.

![alt text][image5]

Click “Next” and click “Create bucket”.

![alt text][image6]

Once the bucket is created, click on the name of the bucket to open the bucket to the contents.

![alt text][image7]

The developer team provides us with the code of the website in the S3 container that we have previously created, which leaves us a compressed file called udacity.zip, this file has been uploaded with the following command from a terminal.

```bash
  aws s3 cp udacity.zip s3://udacity-website-practice2
```

![alt text][image8]

And in the S3 console.

![alt text][image9]

The optional part is to create the key pairs for the jump box machine to access the machines that are in the private network in case you need to access them.

![alt text][image10]

## Deployment script execution steps.

The following images show the status of the steps followed in the deployment of our network and server configuration script for the implementation of the website provided by the development team.

![alt text][image11]

![alt text][image12]

![alt text][image13]

![alt text][image14]

![alt text][image15]

![alt text][image16]

![alt text][image17]

![alt text][image18]

![alt text][image19]

![alt text][image20]

![alt text][image21]

![alt text][image22]

![alt text][image23]

![alt text][image24]

![alt text][image25]

![alt text][image26]

![alt text][image27]

![alt text][image28]

![alt text][image29]

![alt text][image30]

![alt text][image31]


## Website.

This would be the display of our website on the route:  http://Udagr-WebAp-RLD719225CJF-758923361.us-west-2.elb.amazonaws.com

![alt text][image32]
