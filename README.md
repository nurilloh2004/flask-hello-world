# README
#DEPLOYMENT
Deploy application to AWS with GitHub actions and ASG and Zero Downtime

#STRUCTURE
GitHub actions >> ECR >> CodeBuild >> CodePipeline >> ECS + load balancer + ASG

#STEPS
  1. Fork simple application, Dockerize it and create workflow
  2. Get Access keys from AWS IAM and add to secret keys in GitHub (with this keys we can use AWS service with actions)
  3. Create ECR private repository and add to actions with build and push it
  4. in Code Build section we give specific commands to get artifact from ECR in Code pipeline
  5. With Code pipeline we get the new version of image from ECR and deploy to the ECS after build stage
  6. ECS . It gives to us manage our containers with zero downtime and rollback also. If there is some kind of bugs in your code, it will be rollback automatically from ECS. You can show that from CloudWatch and alarms
  7. in ECS section we create task definition manually but only one time because code pipeline in every push to ECR that can create automatically
  8. Load balancer and target group. We choose 2 availability zone with one VPC  and attach target group. In target group we open 80 port to HTTP with root / location, it is a main location that application from Route 53 .
  9. Auto scaling group which we set max 60 % of CPU utilization and 30 % minimum. When it down from 30 % your instances which they create from ASG is down. In this case we gave the desired count for instances.
In real projects, before setting desired tasks we have to show monitoring charts. Depends on it we  know about how many instances need for our application and also with specific times. Example: from 12:00 To 13:30 - desired tasks 5, after this time max Desiree tasks 2



! Stress test

1- ssh to our instance and install that package  and write command based on your plan which you set scaling in ASG
  1.  Install stress in Amazon linux
  2.  sudo amazon-linux-extras install epel -y && sudo yum install stress -y
  3.  stress --cpu 4 --timeout 360
  4.  in this command you can change CPU count and timeout(s)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


Some AWS services in not working perfectly Which they are  CloudWatch, CodeBuild run time, Cluster conteiners with EC2 . Because of this in my AWS account didn't complate (VISA and other credentials).
