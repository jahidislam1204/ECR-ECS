# 🚀 Deploying Docker Applications with Amazon ECR and ECS (EC2)

This guide walks through how to build a Docker image, push it to **Amazon Elastic Container Registry (ECR)**, and deploy it using **Amazon Elastic Container Service (ECS)** with **EC2 launch type**.

---

## 📦 Step 1: Create Repository in Amazon ECR

1. Open the AWS Console and search for **ECR**.
2. Navigate to **Repositories** > **Create repository**.
3. Set a name, e.g., `demo-ecr`.
4. Follow the commands below to push a Docker image:

```bash
# Log in to Amazon ECR Public
aws ecr-public get-login-password --region us-east-1 | \
docker login --username AWS --password-stdin public.ecr.aws/j9i5p7k2

# Build Docker image
docker build -t ecr-demo .

# Tag the Docker image
docker tag ecr-demo:latest public.ecr.aws/j9i5p7k2/ecr-demo:latest

# Push the Docker image to ECR
docker push public.ecr.aws/j9i5p7k2/ecr-demo:latest
```
🖥️ Step 2: Create ECS Cluster (EC2 Launch Type)
Go to ECS > Clusters > Create Cluster.

Choose EC2 Linux + Networking.

Configure the cluster:

Cluster name: ecs-demo

Provisioning model: On-Demand Instance

Instance type: t2.micro

Number of instances: 1

VPC, subnets, and security group: select existing

Container instance IAM role: create a new role if not available

Click Create.

📝 Step 3: Create Task Definition
Go to Task Definitions > Create new Task Definition.

Choose Launch Type Compatibility: EC2.

Name the task: ecs-demo-task-definition.

IAM Role:
Task Role: ecsTaskExecutionRole

If not found, go to IAM Console, search for ecsTaskExecutionRole, and attach the necessary permissions.

Task Settings:
Memory: 300 MiB

CPU: 250

Add Container:
Container name: ecs-demo-container

Image URI: your public ECR image (e.g., public.ecr.aws/j9i5p7k2/ecr-demo:latest)

Port Mappings: 8080:80 TCP

Click Create and review the JSON if needed.

⚙️ Step 4: Create ECS Service
Navigate to your ECS Cluster > Create Service.

Settings:

Launch Type: EC2

Task Definition: ecs-demo-task-definition

Service name: ecs-demo-service

Service type: Replica

Number of tasks: 1

Placement templates: AZ Balanced Spread

Load balancer: None

Click Create Service.

✅ Result
After a few minutes, your Docker container will be running on an EC2 instance managed by ECS. You can access the app using the Public IP of the EC2 instance on port 8080.

🧠 Notes
Make sure security groups allow inbound traffic on port 8080.

You can monitor container logs from ECS > Task > Logs (CloudWatch).

🧰 Technologies Used

Amazon Elastic Container Registry (ECR)

Amazon Elastic Container Service (ECS)

EC2 Instances

Docker

