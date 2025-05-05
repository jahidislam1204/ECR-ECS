# Deploying Docker Applications with Amazon ECR and ECS (EC2)




ECR(Elastic Container Registry) && ECS (Elastic Container Service)


--------------Create Repositories in ECR--------------
 
 -> search ECR
 -> Go to Repositories
 -> Create Repositories (demo-ecr)
 -> To push a docker-image run these command on cli and must be login into your local machine

. aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/j9i5p7k2
. docker build -t ecr-demo 
. docker tag ecr-demo:latest public.ecr.aws/j9i5p7k2/ecr-demo:latest
. docker push public.ecr.aws/j9i5p7k2/ecr-demo:latest



---------------------- Create a cluster on EC2 ------------------
-> select EC2 Linux + Networking
-> Configure cluster
            - Cluster name (ecs demo)
			- provisioning model (on-demand Instance)
			- EC2 instance type (t2.micro)
			- number of instance (1)
			- select vpc,subnets,security group those already created
			- Container instance IAM role (create new role)
-> create			
			
---------------------- Create Task Defination --------------------

-> Create new task defination
-> select launch type compatibilty(EC2)
-> Task Defination name (ecs-demo-task-defination)
           . check the given permission in IAM console 
		   . if not given go to the IAM console
		   . search "ecsTaskExecutionRole"
		   . select it and add permission
-> Task Role ("ecsTaskExecutionRole" That already created)
-> Task execution role ("ecsTaskExecutionRole" That already created)
-> memory (300)
-> cpu(250)
-> add a container 
               . name (ecs-demo-container)
			   . image (copy-paste the image url that already created in ECR eg. 442417563.dkr.ecr.ap-south-1.amazonaws.com/ecr-demo)
			   . port mappings (8080:80 tcp)
			   . rest of the settings remain same
			   
-> create
-> check json file




---------------------- Create Service --------------------

-> create a service
-> Launch type EC2
-> select Task Defination (ecs-demo-task-defination)
-> service name (ecs-demo-service)
-> service type (replica)
-> number of task (1)
-> placement templates (AZ Balanced Spread)
-> Load balance (none)
-> rest of the settings remain same
-> create service


 Result
After a few minutes, your Docker container will be running on an EC2 instance managed by ECS. You can access the app using the Public IP of the EC2 instance on port 8080.

 Notes
Make sure security groups allow inbound traffic on port 8080.

You can monitor container logs from ECS > Task > Logs (CloudWatch).

 Technologies Used

Amazon Elastic Container Registry (ECR)

Amazon Elastic Container Service (ECS)

EC2 Instances

Docker

