AWS 

Overview:
This repository contains the design with draw.io as pdf and explanation about each and every component.
-----------------------------------------------------------------
**1.UI(Frontend)Architecture Design**
Question: Howwe can deploy and scale a UI Application (e.g, React,Angular,Vue).


  **Answer:**

we can host a application with the help of Amazon s3 ,cloudfront and Route 53.

**HOSTING:**

Amazon s3 will be  the best option for hosting the static files and Cloudfront as CDN for global caching and faster delivery.

Amazon s3:

An Amazon S3 bucket is a cloud storage container in AWS used to store files such as images, videos, documents—or even entire websites. It provides highly durable, scalable, and cost-efficient object storage.

Cloud Front:
Amazon CloudFront is a Content Delivery Network (CDN) service from AWS. It speeds up delivery of the website or application by caching content at servers located globally

Route 53:
Amazon Route 53 is AWS’s DNS (Domain Name System) service. It connects user-friendly domain names (example: amazon.com) to the AWS resources

**ROUTING:**
* * Cloudfront routes traffic to s3 so since the UI is statless,scaling can be handled by s3 automatically
* For better performace we are enable cloudfront caching and gzip compression for assets
* Best practice in aws for zero outage we can deploy the application in multiple AWS regions for redundancy and avoid low latency
* for security we cab block pubic access to s3 for that OAC would be the right choice and in cloudfront we can attach AWS Web application Firewall and ACM cerificates as well.

  
    ----------------------------------------------------------------------------------------------------------------------------------------------------------
  2.API (Backend) Architecture Design


  Where API Runs: Amazon ECS with Fargate

  * It's serrverless container platform for codes like Node,python,Java
  * No EC2 management tasks run in private subnets
  * For micoservices we can use aws lambda and EC2 auto scaling
 
  How Horizontal Scaling Works:

  **ECS Service Auto Scaling:**

  It will scale out automatically when cpu memory touch 60 percentage and scale in when CPU memory is in 30%

  * Maintain minimum task acorss mulitple available zones for horizontal availbility.
  * Amazon Load Balancer distributes traffffic evenly accross healthy tasks.

 How the API communicates with Database and traffic enters the Backend:

 * ECS task in private subnets connect to Amazon RDS.
 * No public access to RDS and TLS will take care for DB connections.
 * For proper autoscaling we can use ECS service integrated with cloudwatch metrics.
   ----------------------------------------------------------------------------------------------------------------------------------------------------------------

3**. Database Architecture Design******

How Scaling is Handled

Read Replicas:

*  create replicas in different AZ for read heavy workloads

Vertical Scaling:

* We can increase instance size when CPU is full
* this consider as best practice to handle sudden spikes in heavy workloads

Multi AZ :

* We can eanble multiple AZ deploymentfor high availability and zero productivity outage.


**Backup Plan:**

we can enable daily automated backups and will set retention period as 2 weeks  and will take screenshots.

Security:

WE can encrypt the data and enable KMS encryption and TLS in transit
It would be nice if RDS in private subnet and no public endpoint
we can create security groups

Monitoring:

CouldWatch metrics and will set alarms for failover events (SNS)

--------------------------------------------------------------------------------------------------------------------------------------------------------------------


4. CI/CD Pipeline Design

**Trigger Events:**

We can pull request to main to run build and test for validations
To trigger full pipeline we can merge to main 

build -> test -> deploy to dev -> stage - > production.


**Build Stage:**

we can pull code from github and install if any dependencies needed.
we need UI for static files and API for docker image and tag.

**Testing Workflow:**

Usually they are two testing one is UAT and SIT Testing

UAT means final check by end users  and another is system testing to check whether modules runs in different software without any error.

**Deployment Stage**

UI:

* Upload build artifacts to s3 and invalidate cloudfront cache for updated assets.

API:

* We can push docker image to amazon ECR and authenticate usingOIDC for github actionns

5. Basic Health Check:

   * Need to verify Cloudfront distribution and ALB target group health check and will configure cloudwatch for any failure.
  
  

--------------------------------------------------------------------------------------------------------------------------------------------------------------------


  



   
