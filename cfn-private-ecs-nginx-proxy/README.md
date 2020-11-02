AWS Cloudformation template to create 
 - A Keypair using Lambda function and crhelper scripts.
 - keys are stored in S3 bucket in your account
 - KeyPair is used to launch the instances using Launch templates
 - A VPC 
 - 3 seperate zones for Public, private and Isolated.(each with 3 subnets)
 - A LaunchTemplate to launch WEB servers (httpd on port 80) and SQUID Proxy (running on 3128 port) on PUBLIC Subnet
 - An ALB to expose the Web Servers HTTP port 80
 - A NLB to direct traffic to proxy server installedo n port 3128
 - An ECS Cluster in Private subnet connected thorugh Proxy created in steps above. 
 - A task definiton and service, to run a simple NGINX server using niginx:latest image on ECS cluster.
 
NOTE: This is to be used only in us-east-1 region at this time.

Pre-requisites:
1) Create 2 S3 buckets 
  a) Bucket to store all the templates - Copy all the CFN template files here.
  b) A bucket to store private ckeys created through CFN. Make sure these are NOT public. 
2) Download the Lambda function "crhelper-ec2-keypair.zip" and create the Lambda function in the us-east-1 region. 
3) Download the stack template file and update the parent/root template file "ecs-proxy-parent.yaml" with the correct S3 urls with bucker names created in step 1a


Steps to launch the CFN stack:
1) Upload the updated stack templates to your S3 bukcet
2) Launch the CFN template ussing the parent/root stack file ecs-proxy-parent.yaml
3) Provide valid parameters
     PubPrivateVPCClassB : 'Class B of VPC (172.XXX.0.0/16)'
     KeyPairName : The key pair name to use to connect to the EC2 instances.
     S3BucketName :  Name of your S3 bucket to store your EC2 private keys.
     ECSClusterName : Name of your Amazon ECS Cluster
 4) Verify the resouces created by all child stack and verify ECS cluster with instances.
 5) Verify the Nginx task running in your ECS cluster and access it using the ALB created by "ECSLoadBalancer"
 
 
 
Known Issues: 

1) If you do nto see ECS cluster with instances, NLB used for proxy may not have the instances registerd. 
   Fix: Open the NLB targetgroup and register the instances with tag "WebInstance". Once registerd, terminate ECS instances and wait for ECS Launch tempalte to create new instances. After few minutes you can verify if the the ECSCluster has instances register and nginx task running.Y 
     
     
     
     
     
     
