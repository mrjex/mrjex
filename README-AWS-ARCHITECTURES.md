# AWS Architectures


- Table of Contents
  - [Aktiv Framtid (Startup)](#aktiv-framtid-startup)
    - [Base Architecture](#base-architecture)
    - [Architecture V1](#architecture-v1)
    - [Architecture V2](#architecture-v2)
  - [Individual Projects](#individual-projects)
    - [AWS Sagemaker System](#aws-sagemaker-system)
    - [AWS Big Data System](#aws-big-data-system)
    - [Generative AI Bedrock](#generative-ai-bedrock)
    - [Real-Estate Price Prediction](#real-estate-price-prediction)


## Aktiv Framtid (Startup)


### Base Architecture

![aktiv-framtid-base-architecture](readme-material/aws-architectures/1-aktiv-framtid-base-arhcitecture.PNG)


### Architecture V1

![aktiv-framtid-architecture-v1](readme-material/aws-architectures/2-aktiv-framtid-architecture.PNG)


### Architecture V2

Note that **Swish** and **BankID** are two famous payment methods in Sweden, which are integrated in the dockerized containers pushed to ECR. As I implemented the architecture, I was faced with two options:

1. Use AWS Application Load Balancer to automatically map the public IPs connecting the API Gateway and the REST APIs. This approach would be smoother in terms of setting up and maintaining the system. However, the major drawback was the service's costs, which the startup founder of *Aktiv Framtid* didn't like

2. Automate process of finding the dynamically changing public IPs (as it changes after each deployment of ECS Fargate task definitions)

Due to the stakeholder's strong interest in avoiding costs, me and the engineering team proceeded with the latter approach. For this, we had to emulate the functionality of an Application Load Balancer, in the sense of adding a mechanism for DNS self-discovery. After thorough research, the issue was solved by incorporating the following:

- **Cloudmap:** Map to the ECS Fargate cluster using namespaces for the ECS `rest-api` and `backend-service` task definitions

- **Route 53:** Domain Name registration and management

- **Shell Script inside ECS container:** When the Docker container starts in ECS, it runs a shell script to retrieve the task's dynamic IP address. If the IP is found, the script updates a stage variable in API Gateway, setting the destination URL to point to the specific ECS Fargate container.

- **API Gateway:** Setting *IAM Roles* to allow the specific ECS container to modify stage variables of another service during execution

![aktiv-framtid-architecture-v2](readme-material/aws-architectures/3-aktiv-framtid-architecture.PNG)


## Individual Projects

### AWS Sagemaker System

![sagemaker-architecture](readme-material/aws-architectures/sagemaker-architecture.png)


**Repository:** https://github.com/mrjex/AWS-Sagemaker-System


### AWS Big Data System

![bigdata-architecture](readme-material/aws-architectures/bigdata-architecture.png)

**Repository:** https://github.com/mrjex/AWS-BigData-System


### Generative AI Bedrock

![generative-ai-architecture](readme-material/aws-architectures/generative-ai-architecture.PNG)

**Repository:** https://github.com/mrjex/AWS-Generative-AI-Endpoint/tree/main


### Real-Estate Price Prediction

![real-estate-price-prediction](readme-material/aws-architectures/real-estate-price-prediction-architecture.png)

**Repository:** https://github.com/mrjex/Real-Estate-Price-Prediction