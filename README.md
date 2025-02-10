# Vercel Clone

This project is a **Vercel Clone** built using multiple microservices. It allows users to build, deploy, and serve static assets using AWS services like ECS, ECR, and S3. The project architecture consists of an **API Server**, **Build Server**, **Reverse Proxy**, and **Socket Server**.

## Services Overview

- **api-server**: HTTP API server that handles REST API requests and manages tasks.
- **build-server**: Docker service responsible for cloning Git repositories, building the project, and pushing the build to an S3 bucket.
- **s3-reverse-proxy**: Acts as a reverse proxy for serving static files from the S3 bucket.

## Setup Guide

### Prerequisites
- Node.js installed on your local machine.
- Docker installed and running.
- AWS CLI configured with permissions for ECS, ECR, and S3.

### Local Setup
1. Clone the repository:
    ```sh
    git clone https://github.com/yourusername/vercel-clone.git
    cd vercel-clone
    ```
2. Run `npm install` in each of the services (`api-server`, `build-server`, and `s3-reverse-proxy`):
    ```sh
    cd api-server
    npm install
    cd ../build-server
    npm install
    cd ../s3-reverse-proxy
    npm install
    ```

3. **Docker Build**: Build the `build-server` and push the image to AWS ECR.
    ```sh
    docker build -t build-server .
    aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <your-ecr-url>
    docker tag build-server:latest <your-ecr-url>/build-server:latest
    docker push <your-ecr-url>/build-server:latest
    ```

4. **API Server Setup**: Configure the API server by providing the required configuration such as:
   - `TASK ARN`
   - `CLUSTER ARN`
   - AWS S3 bucket details.

5. Start the services:
    - `api-server`:  
      ```sh
      cd api-server
      node index.js
      ```
    - `s3-reverse-proxy`:  
      ```sh
      cd s3-reverse-proxy
      node index.js
      ```

At this point, the following services will be running:

| S.No | Service         | Port  |
|------|-----------------|-------|
| 1    | api-server      | :9000 |
| 2    | socket.io-server| :9002 |
| 3    | s3-reverse-proxy| :8000 |

## AWS Setup

- **AWS ECS**: Deploy and manage the Docker containers for `api-server`, `build-server`, and `s3-reverse-proxy`.
- **AWS S3**: Store and serve static assets (e.g., `index.html`, `style.css`, etc.).
- **AWS ECR**: Manage Docker images for the build server.

## Project Structure
```
vercel-clone/
│
├── api-server/            # HTTP API Server for REST API requests
├── build-server/          # Docker service to build and push to S3
├── s3-reverse-proxy/      # Reverse Proxy for S3 static assets
└── README.md              # Project documentation
```

## Future Enhancements
- **Authentication and Authorization** for deployments.
- **Custom Domain Mapping**.
- **Improved Logging and Monitoring**.

---

