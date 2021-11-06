# .NET 6 support on AWS

## Overview

This guide describes .NET 6 support provided by AWS services and tools. The guide will be updated as new support is added. 

Related Content:
  * [GitHub home for .NET on AWS](https://github.com/aws/dotnet)
  * [AWS Developer Center - .NET on AWS](https://aws.amazon.com/developer/language/net/)

## Introduction

Customers and AWS teams are excited about the release of .NET 6. Many organizations will want to target .NET 6 because it is a Long Term Support (LTS) release. .
NET 6 applications can already use many AWS services without additional work. For example, it is possible to deploy a new virtual machine instance ([Amazon EC2](https://aws.amazon.com/ec2/)) and by 
adding a command to install the .NET 6 runtime as part of the deployment, the instance will support .NET 6 applications. 

This guide is focused on AWS services and tools that have either been updated, or are being updated, to provide .NET 6 support. While we work to test and validate support for the GA release of .NET 6 to all services and tools, 
there are some steps you can take to use .NET 6 today. These are called out in the sections below. 

Please open an [Issue on this repository](https://github.com/aws-samples/aws-net-guides/issues) with questions about .NET 6 support on AWS. We use this feedback to help us prioritize updates. 


## Compute Services

### Amazon EC2

Customers can install .NET 6 on over 400 [Amazon EC2 instances types](https://aws.amazon.com/ec2/instance-types/). 

The following EC2 [User Data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html#user-data-console) example installs .NET 6 RC2 targeting ARM64 (AWS Graviton2):
```
#!/bin/bash
# Install .NET 6-RC2 SDK for ARM64
sudo apt-get update -y
curl -O https://download.visualstudio.microsoft.com/download/pr/1e7a9f1f-6128-4581-9d72-edfe196320d3/ad3b26879ddaca8b76e16ddddd091d5d/dotnet-sdk-6.0.100-rc.2.21505.57-linux-arm64.tar.gz
mkdir /usr/bin/dotnet
sudo tar -zxvf dotnet-sdk-6.0.100-rc.2.21505.57-linux-arm64.tar.gz -C /usr/bin/dotnet
sudo ln -s /usr/bin/dotnet/dotnet /usr/bin/dotnet
sudo sh -c 'echo "export DOTNET_ROOT=/usr/bin/dotnet" >> /etc/environment'
sudo sh -c 'echo "export PATH=$PATH:$DOTNET_ROOT" >> /etc/environment'
```

In the near future an updated version of the preconfigured Amazon Machine Image (AMI) with [MATE Desktop Environment](https://mate-desktop.org/) that includes .NET 6 will be available. 
Before it is released customers can create a new instance with the current version that includes .NET 5 preinstalled and then install the .NET 6 SDK on that instance. 

[EC2 Image Builder](https://aws.amazon.com/image-builder/) simplifies the building, testing, and deployment of virtual machines and container images for use on AWS or on-premises. 
.NET 6 can be added to images created with Image Builder in two ways:

1. [AWS CloudFormation Template](https://aws.amazon.com/cloudformation/resources/templates/) - A sample Image Builder CloudFormation Template can be used to build an [Ubuntu Server 20 Image with .NET 6](https://github.com/aws-samples/amazon-ec2-image-builder-samples/tree/master/CloudFormation/Linux/ubuntu-with-net6). Currently the template targets .NET 6 Preview but will be updated to .NET 6 GA in the near future. 
1. Image Builder Components - Image Builder will offer Amazon-managed .NET 6 components (runtime and SDK) supporting Windows and Linux distributions in the near future. 



### AWS Elastic Beanstalk

[AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) is an easy-to-use service for deploying and scaling web applications and services developed with .NET and other languages. 
Customers can target both Windows and Amazon Linux 2 platforms to deploy their applications. Use the [self-contained](https://docs.microsoft.com/en-us/dotnet/core/deploying/#publish-self-contained) 
publish option to deploy .NET 6 applications to Elastic Beanstalk. In the near future Elastic Beanstalk will provide managed images that have .NET 6 preinstalled.

### Containers

AWS customers can deploy .NET applications running on either Windows or Linux containers using [Amazon Elastic Container Software](https://aws.amazon.com/ecs/) (ECS) and 
[Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) (EKS). [AWS Fargate](https://aws.amazon.com/fargate/) is a service that you can use to run and manage the 
lifecycle of ECS and EKS containers without the need to manage the container infrastructure yourself

[AWS App Runner](https://aws.amazon.com/apprunner/) is a fully managed service that makes it easy to quickly deploy containerized web applications and APIs, 
scaling up or down automatically to meet application traffic needs. To use with .NET 6 applications, upload an image with the .NET 6 application to 
[Amazon Elastic Container Registry](https://aws.amazon.com/ecr/) (ECR) and use the [source image](https://docs.aws.amazon.com/apprunner/latest/dg/service-source-image.html) support 
to configure AWS App Runner to start, run, scale, and load balance the application. 

### AWS Lambda

[AWS Lambda](https://aws.amazon.com/lambda/) is a serverless, event-driven compute service that can run virtually any type of application or backend service without provisioning or managing servers. 

To get started immediately with .NET 6 on Lambda there are a few options:
1. Container Image - Build a container image with the .NET 6 application packaged inside it. This [PowerShell script](https://github.com/aws/aws-lambda-dotnet/tree/master/LambdaRuntimeDockerfiles) will build a .NET 6 base container image. 
2. Custom Runtime - The custom runtime bundles all necessary .NET 6 libraries in the zip file archive that is deployed to Lambda, see this [example](https://github.com/normj/LambdaNETCoreSamples/tree/master/CustomRuntimeListBucketsNET6). For an ARM64 see this [example](https://github.com/normj/LambdaNETCoreSamples/tree/master/ArmLambdaFunction#net-5-and-6-with-custom-runtimes).

In the near future Lambda will provide managed instances with .NET 6 runtime preinstalled. This will allow customers to deploy .NET 6 applications using zip file archives 
and container images as they do today with .NET Core 3.1 applications. 

In the near future the [AWS Toolkit for Visual Studio](https://aws.amazon.com/visualstudio/) and [AWS Lambda for .NET Core](https://github.com/aws/aws-lambda-dotnet) will offer templates for all of the above options. 
 
## Tools and SDK

### AWS Toolkit for Visual Studio

The [AWS Toolkit for Visual Studio](https://aws.amazon.com/visualstudio/) is an extension for Microsoft Visual Studio on Windows that makes it easier for developers to develop, debug, 
and deploy .NET applications using Amazon Web Services. Visual Studio 2022 supports .NET 6 development, and customers can download the AWS Toolkit for 
[Visual Studio 2022](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.AWSToolkitforVisualStudio2022) from the Visual Studio Marketplace. 
Currently the Toolkit is available as a Preview release. A GA version will be released in the near future, the current status is tracked in this [Issue](https://github.com/aws/aws-toolkit-visual-studio/issues/167). 

### AWS Toolkit for Rider

The [AWS Toolkit for Rider](https://aws.amazon.com/rider/) is an open source plug-in for the [JetBrains Rider](https://www.jetbrains.com/rider/) IDE that makes it easier to create, 
debug, and deploy .NET applications on Amazon Web Services. The Toolkit supports creating a new AWS App Runner service to manage containers, which can host .NET 6 applications. 
Follow the [documentation](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/creating-service-apprunner.html) to setup. Note, select the �ECR/ECR public� option (image source) for .NET applications. 

The Toolkit also provides an option to create a new .NET application that runs on AWS Lambda, including creation of the [AWS Serverless Application Model](https://aws.amazon.com/serverless/sam/) (SAM) template 
used to deploy to Lambda. The Toolkit also provides the ability to deploy a .NET application to Lambda using a SAM template. Matt Ellis details these steps in 
his [blog post](https://blog.jetbrains.com/dotnet/2019/12/09/working-aws-lambda-rider/). Support for deploying .NET 6 applications to Lambda will be available in the near future. 

### AWS Toolkit for Visual Studio Code

The [AWS Toolkit for Visual Studio Code](https://aws.amazon.com/visualstudiocode/) is an open source plug-in for the Visual Studio Code editor that makes it easier to create, 
debug, and deploy applications on Amazon Web Services. The Toolkit supports creating a new AWS App Runner service to manage containers, which can host .NET 6 applications. 
The Toolkit supports building and debugging [AWS Serverless Application Model](https://aws.amazon.com/serverless/sam/) (SAM) templates used to deploy applications to AWS Lambda. 
Support for deploying .NET 6 applications to Lambda will be available in the near future. 

### AWS Toolkit for Azure DevOps

The [AWS Toolkit for Azure DevOps](https://aws.amazon.com/vsts/) is an extension for hosted and on-premises Microsoft Azure DevOps that make it easy to manage and deploy applications to AWS. 
.NET 6 applications can be used with the Toolkit.

## AWS SDK for .NET

The [AWS SDK for .NET](https://github.com/aws/aws-sdk-net) allows .NET developers to integrate AWS services into their application code in a familiar and consistent manner. The library is compatible 
with .NET 6, and is available from [NuGet](https://www.nuget.org/packages/awssdk.core/). Learn how to get started with the 
[AWS SDK for .NET Developer Guide](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/welcome.html). 

## .NET deploymnet tool

The [AWS .NET deployment tool](https://aws.amazon.com/blogs/developer/reimagining-the-aws-net-deployment-experience/) Preview helps customers choose the right AWS service to host a .NET application, 
and guides them through the deployment process. The deployment tool supports targeting .NET 6 for deployments to AWS App Runner, AWS Fargate, and AWS Lambda. Learn more about the 
.NET deployment tool Preview using either the [.NET CLI](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/deployment-tool.html) or the 
AWS Toolkit for Visual Studio [Publish to AWS](https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/publish-experience.html) option.

### Porting Assistant for .NET

The [Porting Assistant for .NET](https://aws.amazon.com/porting-assistant-dotnet/) is an analysis tool that scans .NET Framework applications and generates 
a .NET Core compatibility assessment, helping port .NET Framework applications to Linux faster. The tool quickly scans .NET Framework applications to identify 
incompatibilities with .NET Core, finds known replacements, and generates a detailed compatibility assessment. Both .NET Core 3.1 and .NET 5 are currently supported. 
In the near future customers can target .NET 6. 

## Security and Diagnostics

### AWS Secrets Manager

[AWS Secrets Manager](https://aws.amazon.com/secrets-manager/) helps you protect secrets needed to access your applications, services, and IT resources. 
The service enables you to easily rotate, manage, and retrieve database credentials, API keys, and other secrets throughout their lifecycle. 

The [AWS Secrets Manager Caching Client for .NET](https://github.com/aws/aws-secretsmanager-caching-net) enables in-process caching of secrets and is compatible with .NET 6. 
Please refer to this [blog post](https://aws.amazon.com/blogs/security/how-to-use-aws-secrets-manager-client-side-caching-in-dotnet/) to learn more about how to use 
AWS Secrets Manager client-side caching in .NET.

### AWS X-Ray

[AWS X-Ray](https://aws.amazon.com/xray/) helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture. 
.NET applications can integrate AWS X-Ray with [AWS X-Ray SDK for .NET](https://github.com/aws/aws-xray-sdk-dotnet) and the 
[AWS Distro for OpenTelemetry .NET](https://docs.aws.amazon.com/xray/latest/devguide//xray-dotnet-opentel-sdk.html). Compatibility testing for .NET 6 is underway. 

## DevOps

### AWS CodeBuild

[AWS CodeBuild](https://aws.amazon.com/codebuild/) is a fully managed continuous integration service that compiles source code, runs tests, and produces 
software packages that are ready to deploy. Currently .NET Core 3.1 and .NET 5 are supported on Linux and Windows. 
[Available runtimes](https://docs.aws.amazon.com/codebuild/latest/userguide/available-runtimes.html) are documented with a detailed list of runtime versions and Operating Systems 
supported by CodeBuild. Support for .NET 6 will be added in the near future. To use CodeBuild before .NET 6 support is added, 
create a [custom image](https://docs.aws.amazon.com/codebuild/latest/userguide/sample-docker-custom-image.html) that can be run in CodeBuild. 

### AWS Cloud Development Kit (CDK)

[AWS Cloud Development Kit](https://aws.amazon.com/cdk/) (AWS CDK) is an open source software development framework to define your cloud application resources using familiar programming languages. 
The CDK supports .NET 6. Learn how to get started with these [CDK with C#](https://docs.aws.amazon.com/cdk/latest/guide/work-with-cdk-csharp.html) examples.

## Databases

Build use case-driven, highly scalable, distributed applications suited to your specific needs. AWS offers 
15+ [purpose-built engines](https://aws.amazon.com/products/databases/) to support diverse data models, including relational, key-value, document, in-memory, graph, time series, wide column, 
and ledger databases. Customers can incorporate these managed database instances into their .NET 6 applications using the AWS SDK for .NET. 

### Amazon DynamoDB

[Amazon DynamoDB](https://aws.amazon.com/dynamodb/) is a fully managed, serverless, key-value NoSQL database designed to run high-performance applications at any scale. 
DynamoDB offers built-in security, continuous backups, automated multi-region replication, in-memory caching, and data export tools. The AWS SDK for .NET 
offers several programming models to access DynamoDB. Follow these [examples](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.NET.html) to get started. 
.NET 6 applications are supported. 

[Amazon DynamoDB Accelerator](https://aws.amazon.com/dynamodb/dax/) (DAX) is a fully managed, highly available, in-memory cache for DynamoDB that delivers up to a 10 times 
performance improvement�from milliseconds to microseconds�even at millions of requests per second. Compatibility testing for .NET 6 is 
underway for the [DAX .NET Client](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.client.run-application-dotnet.html). 

### Amazon Quantum Ledger Database (QLDB)

[Amazon Quantum Ledger Database](https://aws.amazon.com/qldb/) (QLDB) is a fully managed ledger database that provides a transparent, immutable, and cryptographically 
verifiable transaction log. The [Amazon QLDB driver for .NET](https://docs.aws.amazon.com/qldb/latest/developerguide/getting-started.dotnet.html) supports .NET 6 and 
other versions of .NET to support applications interacting with data stored in ledgers.