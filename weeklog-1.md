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

Under infrastructure/configure/cookbooks you find chef cookbooks and recipes to install all software required by WeeklogApp to be functional on a ec2 instance.

So far, we have built a CICD pipeline that allows us, with only a couple of clicks, to deploy WeeklogApp in the production environment. Everything looks like the work is done for this application in terms of continuous delivery. 

However, throughtout our CICD pipeline there're several stages were WeeklogApp gets deployed in QA 1 QA 2, UAT and PROD environments. Confusingly, believe it or not, WeeklogApp is deployed in a different fashion on each of above environments.

Even though, all fashions use the same infrastructure/provision scripts, they prepare them differently and all fashions are implemented in separated jenkins jobs.

At this point, you might have identified what the problem is. If we introduce a new change in any provisioining script we don't have certainty it will function the same in every CICD stage as they are used differently.

My challenge this week was to come up with a unified way to deploy WeeklogApp indepedent of the stage. It should only take input parameters, prepare infrastructre/provision scripts and execute them against according aws account.

The output of the challenge was a Jenkinsfile.