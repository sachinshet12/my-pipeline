📘 README: CI/CD Pipeline for Django Notes App using Jenkins, Docker, and Kubernetes
🔧 Project Description
This project implements a complete CI/CD pipeline for a Django-based notes application.
It uses Jenkins to:

Clone the source code from GitHub

Build a Docker image

Push the image to Docker Hub

Deploy the application to Kubernetes


🚀 Technologies Used
Jenkins

Git & GitHub

Docker & DockerHub

Kubernetes

Shell Scripting


🗂️ Project Structure
Copy
Edit
.
├── Dockerfile
├── deployment.yaml
├── service.yaml
└── Jenkinsfile


⚙️ Jenkins Pipeline Breakdown
✅ Stage 1: Clone Code
groovy
Copy
Edit
git url:"https://github.com/sachinshet12/django-notes-app.git", branch: "main"
Clones the latest code from GitHub main branch.

🛠️ Stage 2: Build Docker Image
groovy
Copy
Edit
sh "docker build -t my-note-app ."
Builds a Docker image using the Dockerfile in the repo.

☁️ Stage 3: Push Image to Docker Hub
groovy
Copy
Edit
withCredentials([usernamePassword(...)]) {
    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
    sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
    sh "docker push ${env.dockerHubUser}/my-note-app:latest"
}
Uses Jenkins credentials (ID: dockerHub) to securely log in to DockerHub.

Tags and pushes the image.

⚓ Stage 4: Deploy to Kubernetes
groovy
Copy
Edit
withKubeConfig(credentialsId: 'kubernetes') {
    sh 'kubectl apply -f deployment.yaml'
    sh 'kubectl apply -f service.yaml'
}
Uses withKubeConfig to connect to your Kubernetes cluster (Jenkins credential ID: kubernetes).

Deletes all existing pods and redeploys the updated application.

🔐 Prerequisites
Jenkins with Docker and Kubernetes plugins installed.

DockerHub credentials added in Jenkins (ID: dockerHub)

Kubeconfig credentials added in Jenkins (ID: kubernetes)

A Kubernetes cluster accessible from Jenkins

Dockerfile, deployment.yaml, and service.yaml added to your GitHub repo (e.g., in notesapp/ folder)

📦 Sample Docker Build Command
If testing locally:

bash
Copy
Edit
docker build -t my-note-app .
📤 Sample Docker Push Command (manual)
bash
Copy
Edit
docker tag my-note-app <your_dockerhub_username>/my-note-app:latest
docker push <your_dockerhub_username>/my-note-app:latest
