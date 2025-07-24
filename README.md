# EYEG0-API Deployment on AWS EKS 

This project demonstrates how to containerize a Node.js application, deploy it to AWS using Kubernetes (EKS), and automate the CI/CD pipeline using GitHub Actions.

---

##  Project Overview

We built a simple Node.js API, containerized it using Docker, deployed it to Amazon EKS, and set up an automated deployment workflow with GitHub Actions. This README outlines all the steps followed to complete the deployment task.

---

##  Project Structure

  *  Create a simple web app (Node.js) with an API returning Hello Eyego
  *  Write a Dockerfile and push the image to AWS K8s
  *  Deploy the app on AWS with at least 2 replicas.
  *  Expose via LoadBalancer/Ingress
  *  Automate build, push to A, and deploy to AWS using GitHub Actions, GitLab CI/CD, or Jenkins.
  *  Document steps to migrate the setup to GCB EKS or Alibaba Cloud *

---

## ✅ Task Checklist

### 1. Create a simple web app (Node.js) with an API returning "Hello Eyego"

#### Initialize the Node.js App Locally

1️⃣ Create a project folder

2️⃣ Initialize Node.js project    --> npm install express

3️⃣ Install required dependencies (Express)  --> npm install express

4️⃣ Create the main app file --> app.js file in project repo

5️⃣ Run the app locally   -->  node app.js

      ✅ Expected Output when browse this link locally http://localhost:3000/api  --> Hello Eyego

<img width="539" height="402" alt="image" src="https://github.com/user-attachments/assets/8c7fb31f-6293-4450-a31a-e3bc61d56dde" />

---

### 2.1. Write a Dockerfile and Push the Image to AWS EKS
         to install docker desktop https://www.docker.com/products/docker-desktop/

1️⃣ Create a Dockerfile  --> dockerfile file in project repo

2️⃣ Build the Docker image -->  docker build -t eyego-app .

3️⃣ Run the Docker image locally  -->  docker run -p 3000:3000 eyego-app
    browse this link locally http://localhost:3000/api

4️⃣ Test the img status  -> docker ps  or docker image

### 2.2 Creat repositry in AWS ECR  
      to install aws cli https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html

1️⃣ Login to AWS CLI  --> aws configure :

                                        AWS Access Key ID
                                     
                                        AWS Secret Access Key
                                        
                                        Region →  ex:us-east-1
                                        
                                        Output format → json

2️⃣ Creat ECR repo  -->  aws ecr create-repository --repository-name eyego-app --region \<replace by region id>
                         go to AWS console , copy it's URL 

<img width="948" height="226" alt="image" src="https://github.com/user-attachments/assets/c5ebdfa3-777e-42ee-8f6b-8a4594cd9095" />



3️⃣ Docker Login to AWS ECR  -->  aws ecr get-login-password --region \<replace by region id> | docker login --username AWS --password-stdin \<replace by ecr repo url>

4️⃣ Tag the Docker image before bush to ECR --> docker tag eyego-app:latest \<replace by ecr repo url>

5️⃣ Push the img to ECR -->  docker push \<replace by ecr repo url>
    this img used later to deploy the app using EKS

---

### 3. Deploy the App on AWS EKS with at Least 2






