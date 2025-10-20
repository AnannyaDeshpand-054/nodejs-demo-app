🚀 Automate Code Deployment Using CI/CD Pipeline (GitHub Actions)
🧠 Objective

This project demonstrates how to automate code deployment using a CI/CD pipeline built with GitHub Actions.
The workflow builds, tests, containerizes, and deploys a Node.js web application using Docker.

🛠️ Tools & Technologies
AWS - cloud platform which helps with computing resource (EC2)
GitHub – Version control and workflow automation
GitHub Actions – Continuous Integration & Continuous Deployment (CI/CD)
Node.js – Backend web application framework
Docker – Containerization for consistent app deployment


⚙️ How It Works
**1. Create a ubuntu EC2 instance:**
 - provide name to your instance (cicd_demo)
 - select AMI ubuntu (operating system)
 - select key pair
 - slect VPC
 - configure security group (keep ports 80,22,8000,8080,443 open)
 - configure storage capacity (15GiB - 20 GiB)
 - launch instance

<img width="1247" height="733" alt="image" src="https://github.com/user-attachments/assets/da33c423-18d2-4db6-8f3e-670e8ab532b6"/>

<img width="1242" height="659" alt="image" src="https://github.com/user-attachments/assets/1de84551-9aa1-4a4f-8abb-dc26dbcb7452"/>

<img width="1222" height="777" alt="image" src="https://github.com/user-attachments/assets/70794c91-9f57-40e5-91b9-c31f9cb17476"/>

<img width="1894" height="738" alt="image" src="https://github.com/user-attachments/assets/a739a447-2c14-4a13-ae70-9a93021b8478"/>

<img width="1567" height="290" alt="image" src="https://github.com/user-attachments/assets/8171d991-fc68-4ea8-969e-01ed1ce16747"/>



**2. Install jenkind and Docker in EC2:**
 - SSH into your instance
 - Install java
 - Install jenkins

```
sudo apt install -y fontconfig openjdk-17-jdk
java --version
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian binary/ | \
    sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update -y
sudo apt install -y jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```
 - install Docker
   ```
    sudo apt-get install docker.io
    sudo usermod -aG $USER && jenkins
    newgrp decker
    docker ps
   ```

 <img width="803" height="144" alt="image" src="https://github.com/user-attachments/assets/febad508-434b-4dd4-8747-072e460cdbee" />

<img width="1287" height="403" alt="image" src="https://github.com/user-attachments/assets/b194767f-e02d-466f-a372-e67c75a675c1" />

<img width="1227" height="723" alt="image" src="https://github.com/user-attachments/assets/e12feb00-3613-43b4-a1c8-c7433ccf5637" />

<img width="747" height="370" alt="image" src="https://github.com/user-attachments/assets/b6161fcd-eb53-4785-a486-abafcf36c70a" />


**3. Create a job in jenkins:**
 - Create a job
 - provide it a name (demo_CICD)
 - check git project and provide the url
 - check GitHub hook trigger for GITScm polling (automatically triggers chage in git repo)
 - code the pipeline
```
pipeline{
    agent any
    stages{
        stage("code"){
            steps{
              git url:"https://github.com/AnannyaDeshpand-054/nodejs-demo-app1",branch:"master" 
              echo " code cloned successfully"
            }
        }
        stage("build"){
            steps{
                sh "docker build -t demo-app:latest ."
                echo "image build successfully"
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerhub-cred",
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag demo-app:latest ${env.dockerHubUser}/demo-app:latest"
                sh "docker push ${env.dockerHubUser}/demo-app:latest "
                        
                    }
                echo "code pushed successfully"
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose up -d"
                echo "application deployed successfully"
            }
        }
    }
    
}
```
 - save and build the pipeline

<img width="1341" height="721" alt="image" src="https://github.com/user-attachments/assets/0d7975c2-0f66-487e-9688-773a2a79295d" />

<img width="940" height="340" alt="image" src="https://github.com/user-attachments/assets/13e9e787-ab17-4863-85dc-3460c1795ce3" />

<img width="1035" height="664" alt="image" src="https://github.com/user-attachments/assets/4a0536bf-bb1a-440e-af61-13a417590b5c" />

**step code**
This step will clone the code from give url and branch into the EC2 instance.
<img width="1828" height="151" alt="image" src="https://github.com/user-attachments/assets/d2e06d6d-5814-41b2-b9a1-626b3d0bbb63" />

**step build**
This step will build a image using Dockerfile 
```
# Node Base Image
FROM node:12.2.0-alpine

#Working Directry
WORKDIR /node

#Copy the Code
COPY . .

#Install the dependecies
RUN npm install
RUN npm run test
EXPOSE 8000

#Run the code
CMD ["node","app.js"]
```
**step push**
- This step creates crendiatials,logins and pushes the image to your docker Hub account
- To make the workflow secure, configure the following secrets in your repository:
    Secret Name	Description
    DOCKER_USERNAME	Your Docker Hub username
    DOCKER_PASSWORD	Your Docker Hub access token/password
- Path: to your repo → Settings → Secrets and variables → Actions → New repository secret
- 
  <img width="1542" height="386" alt="image" src="https://github.com/user-attachments/assets/25067d8c-8037-4321-ac86-3ed8bc9975d8" />
  
  <img width="1640" height="254" alt="image" src="https://github.com/user-attachments/assets/d0f6dc63-8fb4-4468-851f-11dac5b5954d" />

**step Deploy**
  - This step will create containers using image that has been pushes in dockerHub (using docker compose command)
      {docker command "docker run -d -p 8000:8000 demo-app:latest" can also be used }
  - This step will finally deploy application in EC2 and will be visible at its public IP address.

    <img width="1920" height="565" alt="image" src="https://github.com/user-attachments/assets/e5ad917c-dd47-4f8b-8f7f-4f44328e625d" />

**BUILD AND RUN YOUR PIPELINE**

<img width="1037" height="794" alt="image" src="https://github.com/user-attachments/assets/88c5221a-2f11-42e9-8779-a4deccf79d57" />
Visit http://http://16.16.253.119:8000/:8000
 to view your running app.

OUR PIPELINE WORKS PERFECT!



✅ Expected Output

After each successful push to master:(create a git webhook for your repo)

GitHub Actions workflow executes automatically.

Docker image gets built and pushed to your Docker Hub repository.

Application deployment becomes automated and consistent.

🧾 Deliverables

A GitHub repository containing:

Working Node.js app

Functional Dockerfile

GitHub Actions workflow (.yml)

This README.md documentation

👩‍💻 Author

Anannya Deshpande
Cloud & DevOps Enthusiast | AWS Certified Cloud Practitioner
📧 Email: anannyamd1809@gmail.com






