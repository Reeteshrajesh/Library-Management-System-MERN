## Library Management System 📚

A full-stack web application built on the **MERN Stack** (MongoDB, Express, React, and Node.js) for managing library activities such as adding books, managing members, issuing and returning books, and more.
![1](https://user-images.githubusercontent.com/73348574/205624307-6a1b18fa-5ef7-4de9-b141-9225eca62c6c.png)


## Index ✏️

- [Library Management System 📚](#library-management-system-)
- [Index ✏️](#index-️)
- [Features of LCMS 🚀](#features-of-lcms-)
- [Setup 🔥](#setup-)
  - [Frontend Setup 🍧](#frontend-setup-)
  - [Backend Setup 🍿](#backend-setup-)
- [Dockerization 🐳](#dockerization-)
  - [Dockerize Frontend](#dockerize-frontend)
  - [Dockerize Backend](#dockerize-backend)
  - [Docker Compose for Full Application](#docker-compose-for-full-application)
- [Deployment Instructions 🚀](#deployment-instructions-🚀-)
- [Technologies Used 🛠](#technologies-used-)
- [Screenshots](#Screenshots-)
- [Author 📝](#author-)
- [Connect Me On 🌍](#connect-me-on-)
- [Key Features of Technology Used 🏆](#Key-Features)

---

## Features Of LCMS 🚀

- Admin Login and Student Login
- Admin and Student Dashboard
- Adding Library Members
- Adding Books with Available Copies
- Issue and Return Transaction tracking of a Book by the Member
- Reserving a book for specific dates
- Showing the Achievements, Event Gallery

--- 

## Setup 🔥

- Fork the Repo

- Clone the repo to your local machine
  `git clone <repo-url>`

### Frontend Setup 🍧

1. Get into the chatapp directory
   `cd frontend`

2. Run `yarn` to install dependencies

3. Create a `.env` file and create variables as mentioned in the `.env.example` with the values

4. Run `yarn start` to start the application

### Backend Setup 🍿

1. Get into backend directory `cd backend`

2. Run `yarn` to install dependencies

3. Create a MongoDB account and get the MONOGO_URL for connecting the server and the Database

4. Create a `.env` file and create variables as mentioned in the `.env.example` with the values

5. Run `nodemon server.js` to start the server [Should have installed nodemon globally]


--- 

## Dockerization 🐳

#### Dockerize Frontend
Build and run the image:

    
        docker build -t library-management-frontend ./frontend
        docker run -d -p 3000:3000 library-management-frontend

#### Dockerize backend
Build and run the image:

       
          docker build -t library-management-backend ./backend
          docker run -d -p 5000:5000 library-management-backend


### Docker Compose for Full Application

Run the application using Docker Compose:

          docker-compose up --build

---
---

# Deployment Instructions 🚀
### Step 1: Setup Jenkins for CI/CD

1. **Create a Jenkins job to build the application:**
   - Go to Jenkins dashboard and create a new pipeline job.
   - In the pipeline configuration, set up the pipeline script (or use `Jenkinsfile` if you have it in your repository).
   - Add a build step to pull the code from the GitHub repository and build the Docker images for both frontend and backend.

2. **Pipeline Script for Jenkins:**
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Checkout Code') {
         steps {
           git 'https://github.com/Reeteshrajesh/library-management-system.git'
         }
       }
       stage('Build Docker Images') {
         steps {
           script {
             // Build frontend Docker image
             sh 'docker build -t library-management-frontend ./frontend'
             // Build backend Docker image
             sh 'docker build -t library-management-backend ./backend'
           }
         }
       }
       stage('Test Docker Containers') {
         steps {
           script {
             // Add your test commands here to validate Docker containers (e.g., health checks)
             sh 'docker run --rm library-management-frontend test'
             sh 'docker run --rm library-management-backend test'
           }
         }
       }
     }
   }


### Step 2: Vulnerability Scanning


To ensure the security of your Docker images, it's important to perform vulnerability scanning. Trivy is a fast and simple vulnerability scanner for Docker images. In this step, we'll install Trivy and integrate it into a Jenkins pipeline to scan your Docker images for vulnerabilities before deployment. If any **high** vulnerabilities are found, the deployment will be blocked.


#### Install Trivy for Docker image scanning:

1. **For macOS (using Homebrew):**
   ```bash
   brew install trivy
   
2. **Alternatively, you can use Docker to install Trivy in a container:**
   ```bash
    docker pull aquasec/trivy

3. **Create a Jenkins job to block deployment if high vulnerabilities are found:**

    - Set up a Jenkins job to run Trivy to scan the Docker images.
    - Configure the Jenkins pipeline to use Trivy for scanning the frontend and backend images   for vulnerabilities before deployment. If any high vulnerabilities are found, stop the deployment.


4.**Pipeline Script for Jenkins (Vulnerability Scanning):**


      
      pipeline {
        agent any
        stages {
          stage('Scan Docker Images for Vulnerabilities') {
            steps {
              script {
                // Scan the frontend image
                sh 'trivy image library-management-frontend:latest'
                // Scan the backend image
                sh 'trivy image library-management-backend:latest'
              }
            }
          }
          stage('Deploy') {
            steps {
              script {
                // Deploy only if no high vulnerabilities are found
                // Add logic to check the Trivy report and proceed accordingly
                echo 'Deploying application to Kubernetes...'
              }
            }
          }
        }
      }



### Step 3: Deploy with Kubernetes and ArgoCD

#### Create a Kubernetes deployment file(deployment.yaml):


    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: library-management
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: library-management
      template:
        metadata:
          labels:
            app: library-management
        spec:
          containers:
            - name: frontend
              image: library-management-frontend:latest
              ports:
                - containerPort: 3000
            - name: backend
              image: library-management-backend:latest
              ports:
                - containerPort: 5000

    


#### Deploy using ArgoCD connected to your Kubernetes cluster.
  - Install ArgoCD and link it to your Kubernetes cluster.
  - In ArgoCD, create an application that connects to the Git repository where your Kubernetes configuration is stored.
  - Use ArgoCD to sync and deploy the library-management application to your Kubernetes cluster.

 ---

## Technologies Used 🛠

- **Frontend**: React.js (Hooks), Yarn
- **Backend**: Node.js, Express.js
- **Database**: MongoDB
- **Deployment**: AWS EC2, Docker, Jenkins, ArgoCD, Kubernetes
- **CI/CD**: Jenkins, Trivy for Vulnerability Scanning

---

## Screenshots

![1](https://user-images.githubusercontent.com/73348574/205623377-999c0de5-6796-4100-85e6-96e3e7d4fb77.png)
![2](https://user-images.githubusercontent.com/73348574/205632416-bfcc2c19-3f70-4688-bb7e-0ccd83be3038.png)
![3](https://user-images.githubusercontent.com/73348574/205632598-6b009820-20ec-4e9f-92bf-00af92d4f1a4.png)
![4](https://user-images.githubusercontent.com/73348574/205632198-d99fcc8d-903d-4b60-9cec-56f8e0716290.png)
![5](https://user-images.githubusercontent.com/73348574/205631397-2793e97e-3cc6-4b60-8ee1-ec81716b9d6d.png)
![6](https://user-images.githubusercontent.com/73348574/205631670-5dcb6437-afb1-4aaf-87d7-b47c3b01d7b1.png)
![7](https://user-images.githubusercontent.com/73348574/205631804-6c631b5e-8bcd-41c4-bb73-bab6ea8b78f7.png)
![8](https://user-images.githubusercontent.com/73348574/205631977-f393ca09-aa24-42a5-9bd7-d92d471c514c.png)

---

## Author 📝

- [@Reeteshrajesh]((https://github.com/Reeteshrajesh))

## Connect Me On 🌍

linkedIn :: https://www.linkedin.com/in/reetesh-kumar-850807255/

---

### Key Features:
- **Setup and Deployment**: Comprehensive steps for local development and deploying to AWS EC2 using Docker, Jenkins, and Kubernetes.
- **Dockerization**: Separate Dockerfiles for frontend and backend, with a Docker Compose option.
- **CI/CD Workflow**: Integrated instructions for Jenkins and Trivy.
- **ArgoCD Deployment**: Easy configuration for Kubernetes.


