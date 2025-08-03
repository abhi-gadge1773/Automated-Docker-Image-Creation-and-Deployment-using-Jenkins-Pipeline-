# 🚀 Automated Docker Image Creation and Deployment using Jenkins Pipeline

This project demonstrates a complete CI/CD workflow that automates the process of:

- Cloning a GitHub repository
- Building a Docker image from a Flask application
- Pushing the image to a **local Docker registry**
- Running the container using a Jenkins pipeline

---

## 📁 Project Structure

```

Project4/
├── app/
│   ├── app.py
│   └── requirements.txt
    |__ Dockerfile
└── Jenkinsfile

````

---

## 🔧 Tech Stack

- Jenkins (Pipeline as Code)
- Docker
- Local Docker Registry
- Python Flask

---

## ⚙️ Jenkins Pipeline Overview

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'localhost:5000/my-app'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/abhi-gadge1773/Automated-Docker-Image-Creation-and-Deployment-using-Jenkins-Pipeline-.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🔨 Building Docker image...'
                sh 'docker build -t $IMAGE_NAME -f app/Dockerfile app'
            }
        }

        stage('Push to Local Registry') {
            steps {
                echo '📦 Pushing Docker image to local registry...'
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying container...'
                sh '''
                    docker stop my-app || true
                    docker rm my-app || true
                    docker run -d -p 80:5000 --name my-app $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
````

---

## ✅ How to Run the Project

### 1. Start Local Docker Registry

```bash
docker run -d -p 5000:5000 --name registry registry:2
```

### 2. Let Jenkins Run the Pipeline

Jenkins will:

* Clone your GitHub repo
* Build Docker image from `app/Dockerfile`
* Push it to `localhost:5000`
* Run the container exposing it to `http://localhost`

### 3. Access the App

Open in browser:

```
http://localhost
```

---

## 📌 Important Notes

* Your Flask app runs on port `5000` inside the container.
* The Jenkins pipeline maps `-p 80:5000` so it's accessible on `http://localhost`
* Use a **production-ready WSGI server** (like Gunicorn) for production deployments.

---

## 🧠 Author

**Abhijeet Gadge**
🔧 DevOps Engineer | Automation Enthusiast
🔗 [LinkedIn](https://www.linkedin.com/in/abhijeetgadge/)
🔗 [GitHub](https://github.com/abhi-gadge1773)

