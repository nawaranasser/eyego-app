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

### 3. Deploy the App on AWS EKS with at Least two and 4. Expose via LoadBalancer
       to install eksctl  https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html

1️⃣ Create EKS cluster using eksctl  --> eksctl create cluster --name eyego-cluster \
  --region us-east-1 \
  --nodegroup-name eyego-nodes \
  --nodes 2 \
  --node-type t3.micro \
  --with-oidc \
  --ssh-access \
  --ssh-public-key <your-ssh-key>

        to create ssh key  --> ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

       if it done correctly , you can see it in cloudformation 
<img width="942" height="326" alt="image" src="https://github.com/user-attachments/assets/c8ceb1d1-5a98-49a0-bfd1-06fea708006c" />

       
2️⃣ Create Kubernetes files --> deployment.yaml and service.yaml  in project repo

3️⃣  Deployment operation -->
                               kubectl apply -f deployment.yaml
                               kubectl apply -f service.yaml
4️⃣ Note the servise.yaml file run 2 replica using loud balancer

5️⃣ Exract the loud balancer external IP  -->  kubectl get service eyego-service

**browse this external ip to see your app run via interner**

<img width="815" height="236" alt="image" src="https://github.com/user-attachments/assets/11816dac-af55-4571-abf1-7f2cd01ebea6" />

6️⃣ Test number of replicas (pods) and its status --> kubectl get pods

7️⃣ Verify the Deployment and Service --> kubectl get deployments
                                         kubectl get pods
                                         kubectl get svc

----

### 5.  Automate build, push, and deploy using CI/CD (GitHub Actions)

1️⃣ Push the full project to GitHub repo

2️⃣ Create auto deploy file -->  deploy.yml in project repo

<img width="248" height="132" alt="image" src="https://github.com/user-attachments/assets/dc526812-7cd9-49b6-8341-0805982b6f3f" />











