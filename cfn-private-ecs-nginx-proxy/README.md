AWS Cloudformation template to create 
 - A VPC 
 - 3 subnets for Public, private and Isolated.(each with 3 subnets)
 - A LaunchTemplate to launch WEB servers (httpd on port 80) and SQUID Proxy (running on 3128 port) on PUBLIC Subnet
 - An ALB to expose the Web Servers HTTP port 80
 - A NLB to direct traffic to proxy server installedo n port 3128
 - An ECS Cluster in Private subnet connected thorugh Proxy created in steps above. 
 - A task definiton and service, to run a simple NGINX server using niginx:latest image on ECS cluster.
 
 
