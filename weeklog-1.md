# weeklog 1

## Unify application deployment across different environments

To respect confidentiality of my employer I will refer to the application as WeeklogApp. Everything I post 
is authored by me and is not revealing confidential information or propietary software owned by my employer.

WeeklogApp is a Rest API written in Java 8. Deployed on a tomcat 8 webserver running on aws ec2 instances.
Stores data in  MySQL 5.6 RDS and it´s consumed by different clients either by HTTP or a java client.

Let´s talk about infrastructure provisioning scripts and tools used to achieved an automated aws deployment.
```
WeeklogApp
|_ java-application
|_ insfrastructure
    bootstrap.sh
    deployer.py
    |_ provision
      |_ template.json
      |_ parameters.json
      |_ tags.json
    |_ configure
      |_ cookbooks
         |_ tomcat
         |_ java
         |_ weblogapp
```    
#### AWS Provisioning
Under insfrastructure/provision you find aws cloudformation template to provision cloud resources with parameters and tags.

**template.json ->** Describes our infrastructure architecture design and defines input parameters to make that infrastructure configurable. 

**parameters.json ->** Separated file with all parameters defined by template.json that allows you to have parameter file per environment.

**tags.json ->** Defines all tags AWS Resources will be tagged as.

#### AWS EC2 Instance configuration 

