Setup Guide
Services Overview
This project consists of the following services:

api-server: Handles deployment requests, fetches Git repositories, and triggers the build process.
build-server: Clones the repository, builds the project, and uploads the output files to AWS S3.
s3-reverse-proxy: Routes subdomains and domains to the correct S3 bucket static assets.
socket.io-server: Manages real-time build progress updates.
Local Setup (Without Docker Compose)
1️⃣ Install Dependencies
Navigate to each service folder and install dependencies:

sh
Copy
Edit
cd api-server && npm install
cd ../build-server && npm install
cd ../s3-reverse-proxy && npm install
2️⃣ Build and Run build-server (Docker)
Since the build-server runs inside a Docker container, manually build and run it:

sh
Copy
Edit
cd build-server
docker build -t build-server .
docker run -d --name build-server build-server
3️⃣ Configure api-server
Before running the api-server, make sure you set up the required AWS environment variables:

sh
Copy
Edit
export TASK_ARN=your-task-arn
export CLUSTER_ARN=your-cluster-arn
Then, start the server:

sh
Copy
Edit
cd api-server
node index.js
4️⃣ Start s3-reverse-proxy
sh
Copy
Edit
cd s3-reverse-proxy
node index.js
Services & Ports
S.No	Service	Port
1	api-server	9000
2	socket.io-server	9002
3	s3-reverse-proxy	8000
Deploying to AWS
Push the build-server image to AWS ECR:
sh
Copy
Edit
docker tag build-server <aws-account-id>.dkr.ecr.<region>.amazonaws.com/build-server
docker push <aws-account-id>.dkr.ecr.<region>.amazonaws.com/build-server
Deploy services using AWS ECS and S3.
