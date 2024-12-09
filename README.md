# Scalable-AWS-Based-Containerized-Application
Scalable AWS-Based Containerized Application

The system architecture is a modular, event-driven, scalable deep learning (DL) image-processing pipeline deployed on AWS, consisting of three main containers: FastAPI server, image processing, and results retrieval.

- **FastAPI Server**: A containerized FastAPI server handles client image uploads, stores the files in S3, and sends metadata to an SQS queue.

- **Image Processing**: When SQS transitions from empty to having messages, an EventBridge event triggers a standalone ECS task running a containerized image processor. The processor handles image processing asynchronously using OpenCV or deep learning techniques, calculates relevant results (such as image area), and stores the processed data in DynamoDB.

- **Results Retrieval**: A standalone Results Retrieval ECS task is used for post-processing.

The architecture is designed to be highly scalable, fault-tolerant, and cost-efficient, with components working independently to ensure seamless communication and processing.
