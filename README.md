# 🚀 Jenkins CI/CD Deployment Portal

A professional DevOps deployment portal demonstrating an automated **CI/CD pipeline using GitHub, Jenkins, Docker, and Flask**.

The project automatically builds, tests, and deploys the application whenever new code changes are pushed to GitHub.

---

## 📌 Project Overview

This project demonstrates how a DevOps engineer can automate the application deployment process.

Instead of manually building and deploying the application, Jenkins automatically performs the following steps:

```text
Developer
    ↓
GitHub
    ↓
Jenkins
    ↓
Checkout
    ↓
Docker Build
    ↓
Application Test
    ↓
Docker Deploy
    ↓
Running Application
```

---

## 🛠️ Technologies Used

* **Python**
* **Flask**
* **Gunicorn**
* **Docker**
* **Jenkins**
* **Git**
* **GitHub**
* **Linux**
* **Jenkinsfile**
* **CI/CD**

---

## 🏗️ Architecture

```text
                 ┌─────────────────┐
                 │    Developer    │
                 │   Git Push      │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │     GitHub      │
                 │ Source Code     │
                 └────────┬────────┘
                          │
                    Poll SCM
                          │
                          ▼
                 ┌─────────────────┐
                 │     Jenkins     │
                 │   CI/CD Server  │
                 └────────┬────────┘
                          │
             ┌────────────┼────────────┐
             ▼            ▼            ▼
        ┌─────────┐  ┌─────────┐  ┌─────────┐
        │Checkout │  │  Build  │  │  Test   │
        └─────────┘  └─────────┘  └─────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │ Docker Image    │
                 │ jenkins-        │
                 │ deployment-     │
                 │ portal:latest   │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │ Docker Container│
                 │ Port: 5002      │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │ Deployment      │
                 │ Portal          │
                 │ localhost:5002  │
                 └─────────────────┘
```

---

## 🔄 CI/CD Pipeline

The Jenkins pipeline is defined in the `Jenkinsfile`.

### 1. Checkout

Jenkins retrieves the latest source code from the GitHub repository.

```groovy
checkout scm
```

### 2. Build

Jenkins builds a Docker image from the application source code.

```bash
docker build -t jenkins-deployment-portal:latest .
```

### 3. Test

A temporary Docker container is started and the application health endpoint is tested.

```bash
curl -f http://localhost:5003/health
```

The health endpoint returns:

```json
{
  "status": "healthy",
  "application": "DevOps Deployment Portal"
}
```

### 4. Deploy

Jenkins stops and removes the previous application container and starts a new version.

```bash
docker run -d \
  --name jenkins-deployment-portal \
  -p 5002:5000 \
  --restart unless-stopped \
  jenkins-deployment-portal:latest
```

---

## ⚙️ Automated Trigger

The Jenkins job uses **Poll SCM** to automatically check the GitHub repository for new changes.

Current schedule:

```text
H/2 * * * *
```

When a new commit is detected:

```text
Git Push
   ↓
Jenkins detects change
   ↓
Pipeline starts automatically
   ↓
Build
   ↓
Test
   ↓
Deploy
```

---

## 🐳 Docker Configuration

The application is packaged as a Docker image.

### Dockerfile

```dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
```

The application runs inside the container on port:

```text
5000
```

The host exposes it on:

```text
5002
```

Therefore the application is accessible at:

```text
http://localhost:5002
```

---

## ❤️ Health Check

The application provides a health endpoint:

```text
GET /health
```

Example response:

```json
{
  "status": "healthy",
  "application": "DevOps Deployment Portal"
}
```

This endpoint is used by Jenkins during the testing stage.

---

## 📂 Project Structure

```text
jenkins-deployment-portal/
│
├── app.py
├── Dockerfile
├── Jenkinsfile
├── requirements.txt
├── .gitignore
├── README.md
│
├── templates/
│   └── index.html
│
└── static/
```

---

## 🚀 Run the Application Manually

Clone the repository:

```bash
git clone https://github.com/maryamkhanum0632/jenkins-deployment-portal.git
```

Enter the project:

```bash
cd jenkins-deployment-portal
```

Build the Docker image:

```bash
docker build -t jenkins-deployment-portal:latest .
```

Run the container:

```bash
docker run -d \
  --name jenkins-deployment-portal \
  -p 5002:5000 \
  --restart unless-stopped \
  jenkins-deployment-portal:latest
```

Open:

```text
http://localhost:5002
```

Health check:

```text
http://localhost:5002/health
```

---

## 📊 Jenkins Pipeline Stages

```text
┌──────────┐
│ Checkout │
└────┬─────┘
     ↓
┌──────────┐
│  Build   │
└────┬─────┘
     ↓
┌──────────┐
│   Test   │
└────┬─────┘
     ↓
┌──────────┐
│  Deploy  │
└────┬─────┘
     ↓
┌──────────┐
│   Live   │
│   App    │
└──────────┘
```

---

## 🎯 Key DevOps Concepts Demonstrated

### CI/CD

Automates the process of integrating, testing, and deploying application changes.

### Pipeline as Code

The CI/CD workflow is stored in the repository as a `Jenkinsfile`.

### Containerization

Docker packages the application and its dependencies into a portable container.

### Automated Testing

Jenkins verifies that the application health endpoint is working before deployment.

### Automated Deployment

A successful pipeline automatically deploys the latest Docker image.

### Source Control

Git and GitHub are used to manage and version the project source code.

### SCM Polling

Jenkins periodically checks GitHub for new commits and automatically starts the pipeline when changes are detected.

---

## 💼 Interview Explanation

> I developed a Flask-based Deployment Portal and created a Jenkins CI/CD pipeline integrated with GitHub. When code changes are pushed to GitHub, Jenkins automatically detects the change, checks out the source code, builds a Docker image, runs an application health test, and deploys the updated application as a Docker container.

---

## ✅ Project Result

The project demonstrates a complete local CI/CD workflow:

```text
GitHub
   ↓
Jenkins
   ↓
Build
   ↓
Test
   ↓
Docker
   ↓
Deploy
   ↓
Running Application
```

The pipeline has been successfully tested with automated builds and deployments.

---

## 👩‍💻 Author

**Maryam Khanum**

DevOps Learner | Docker | Jenkins | GitHub | Linux | CI/CD
