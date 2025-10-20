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
**1. Create a ubuntu EC2 instance**
 - provide name to your instance (cicd_demo)
 - select AMI ubuntu (operating system)
 - select key pair
 - slect VPC
 - configure security group (keep ports 80,22,8000,8080,443 open)
 - configure storage capacity (15GiB - 20 GiB)
 - launch instance

<img width="1247" height="733" alt="image" src="https://github.com/user-attachments/assets/da33c423-18d2-4db6-8f3e-670e8ab532b6" />

<img width="1242" height="659" alt="image" src="https://github.com/user-attachments/assets/1de84551-9aa1-4a4f-8abb-dc26dbcb7452" />

<img width="1222" height="777" alt="image" src="https://github.com/user-attachments/assets/70794c91-9f57-40e5-91b9-c31f9cb17476" />

<img width="1567" height="290" alt="image" src="https://github.com/user-attachments/assets/8171d991-fc68-4ea8-969e-01ed1ce16747" />

<img width="1894" height="738" alt="image" src="https://github.com/user-attachments/assets/a739a447-2c14-4a13-ae70-9a93021b8478" />

**2. Install jenkind and Docker in EC2**
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
   

GitHub Actions automatically triggers the workflow (.github/workflows/ci-cd.yml).

It installs dependencies using npm install.

Runs test scripts (if defined).

Builds a Docker image for the application.

🚀 2. Continuous Deployment (CD)

After a successful build:

The Docker image is tagged and pushed to Docker Hub (or GitHub Container Registry).

The app can then be deployed to a server, container service, or cloud platform (e.g., AWS, Azure, or GCP).

🧩 Sample GitHub Actions Workflow

Below is an example ci-cd.yml file used in this project:

name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

      - name: Build Docker image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/node-web-app:latest .

      - name: Log in to Docker Hub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin

      - name: Push Docker image
        run: docker push ${{ secrets.DOCKER_USERNAME }}/node-web-app:latest

🔐 Required GitHub Secrets

To make the workflow secure, configure the following secrets in your repository:

Secret Name	Description
DOCKER_USERNAME	Your Docker Hub username
DOCKER_PASSWORD	Your Docker Hub access token/password

Path:
Go to your repo → Settings → Secrets and variables → Actions → New repository secret

🧪 How to Run Locally
# Clone the repository
git clone https://github.com/<your-username>/<your-repo-name>.git

# Navigate into the project
cd <your-repo-name>

# Install dependencies
npm install

# Run the app
npm start


Visit http://localhost:3000
 to view your running app.

📦 Docker Build (Manual)

If you want to test containerization manually:

docker build -t node-web-app .
docker run -p 3000:3000 node-web-app

✅ Expected Output

After each successful push to main:

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
📧 Email: [Your Email]
🌐 LinkedIn: [Your LinkedIn Profile]

<img width="1247" height="733" alt="image" src="https://github.com/user-attachments/assets/da33c423-18d2-4db6-8f3e-670e8ab532b6" />
<img width="1242" height="659" alt="image" src="https://github.com/user-attachments/assets/1de84551-9aa1-4a4f-8abb-dc26dbcb7452" />
<img width="1222" height="777" alt="image" src="https://github.com/user-attachments/assets/70794c91-9f57-40e5-91b9-c31f9cb17476" />
<img width="1567" height="290" alt="image" src="https://github.com/user-attachments/assets/8171d991-fc68-4ea8-969e-01ed1ce16747" />
<img width="1894" height="738" alt="image" src="https://github.com/user-attachments/assets/a739a447-2c14-4a13-ae70-9a93021b8478" />





<img width="803" height="144" alt="image" src="https://github.com/user-attachments/assets/febad508-434b-4dd4-8747-072e460cdbee" />

<img width="1287" height="403" alt="image" src="https://github.com/user-attachments/assets/b194767f-e02d-466f-a372-e67c75a675c1" />

<img width="1227" height="723" alt="image" src="https://github.com/user-attachments/assets/e12feb00-3613-43b4-a1c8-c7433ccf5637" />

<img width="747" height="370" alt="image" src="https://github.com/user-attachments/assets/b6161fcd-eb53-4785-a486-abafcf36c70a" />


<img width="1357" height="784" alt="image" src="https://github.com/user-attachments/assets/e34324d0-23ad-43d8-a4c1-17a552f42ca7" />
<img width="941" height="385" alt="image" src="https://github.com/user-attachments/assets/e2d42226-3a3c-4052-b9ff-a4092eedb52f" />

<img width="662" height="134" alt="image" src="https://github.com/user-attachments/assets/ddf4eb6e-f23b-4a71-a3a4-177125a90aa8" />



<img width="1641" height="279" alt="image" src="https://github.com/user-attachments/assets/81c21896-2270-4fa2-b2b2-56d6bba8c103" />
<img width="1110" height="818" alt="image" src="https://github.com/user-attachments/assets/ebcb49d4-9211-4898-aa32-5998650e8432" />


<img width="1920" height="451" alt="image" src="https://github.com/user-attachments/assets/36697650-656f-4cb4-a3d9-787e6c98216f" />
