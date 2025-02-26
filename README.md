# Scalable Containerized Deep Learning Image Processing on AWS

### Intro: Scalable AWS-Based Containerized Application

The system architecture is a modular, event-driven, scalable deep learning (DL) image-processing pipeline deployed on AWS, consisting of three main containers: FastAPI server, image processing, and results retrieval.

-    **FastAPI Server**: A containerized FastAPI server handles client image uploads, stores the files in S3, and sends metadata to an SQS queue.

-    **Image Processing**: When SQS transitions from empty to having messages, an EventBridge event triggers a standalone ECS task running a containerized image processor. The processor handles image processing asynchronously using OpenCV or deep learning techniques, calculates relevant results (such as image area), and stores the processed data in DynamoDB.

-    **Results Retrieval**: A standalone Results Retrieval ECS task is used for post-processing.


---


### Architecture:

The architecture is designed to be highly scalable, fault-tolerant, and cost-efficient, with components working independently to ensure seamless communication and processing.

   <img width="599" alt="Screenshot 2025-02-26 at 12 26 50 PM" src="https://github.com/user-attachments/assets/3b167d9f-d5db-4886-87e8-902ecd9ecd3c" />
<img width="808" alt="Screenshot 2025-02-26 at 12 31 12 PM" src="https://github.com/user-attachments/assets/1bd9f007-36e5-4252-a5e7-75bb9b052acb" />


---


### Current Containers:

#### AWS FastAPI Server Container:
1. **Dockerized FastAPI Application:**
   - Tested the Dockerized FastAPI application locally to confirm functionality.
   - Successfully built and tagged the FastAPI application Docker image.

2. **AWS ECR Setup:**
   - Created a private ECR repository (`fastapi-server-image-processing`) for the Docker image.
   - Pushed the Docker image to ECR successfully after resolving IAM permission issues.

3. **AWS ECS and Task Definition:**
   - Created an ECS cluster using AWS Fargate.
   - Defined a task for the FastAPI container with appropriate IAM roles and resource limits, and using the pushed Docker image from ECR.
   - Configured port mappings and networking mode (`awsvpc`).

4. **Service Deployment:**
   - Deployed the ECS task as a service in the cluster.
   - Attached an Application Load Balancer (ALB) to handle traffic to the containerized FastAPI server.

5. **Integration Testing:**
   - Tested the deployed FastAPI service using the client script.
   - Verified connectivity with AWS S3 and SQS services for uploading files and sending metadata.

6. **Auto Scaling and Monitoring:**
   - Configured ECS Service Auto Scaling to handle increasing workloads.
   - Set up monitoring and logging using Amazon CloudWatch.

---

### Next AWS Containers:

#### AWS Image Processing Container:
- Containerize the image processing logic.
- Create a new ECS task definition.
- Configure an EventBridge rule to trigger the ECS task based on SQS message events.

#### AWS Results Retrieval Container:
- Containerize the logic to fetch results from DynamoDB.
- Set up task definitions.
