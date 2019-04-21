# AWS ELastic Container Service Notes

## Fargate

### Running tasks in private subnets without NAT Gateway
- If the Docker image is stored in ECR, two VPC Endpoints must be created to pull the images:
    - a VPC Endpoint for the com.amazonaws.ca-central-1.**ecr.dkr** service (ECR Docker service)
    - a VPC Endpoint for the com.amazonaws.ca-central-1.**s3** service (ECR stores actual Docker images in S3, this is a Gateway, need to add a route to the Subnet route table) 

Note: no need to create a VPC Endpoint to the com.amazonaws.ca-central-1.ecr.**api** or com.amazonaws.ca-central-1.**ecs** services for Fargate tasks. See https://docs.aws.amazon.com/AmazonECR/latest/userguide/vpc-endpoints.html

If your container log driver is 'awslogs' then a VPC Endpoint to Amazon CloudWatch has to be added to:
  - VPC endpoind for the com.amazonaws.ca-central-1.**logs** service
  
Otherwise, your task won't start. Error message being '**DockerTimeoutError: Could not transition to started; timed out after waiting ...**'. See https://github.com/aws/containers-roadmap/issues/48.

