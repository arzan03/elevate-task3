# CI/CD Pipeline for Node.js Web App

This project demonstrates how to set up CI/CD pipelines using GitHub Actions and Jenkins to automate the deployment of a Node.js web application.

---

## Task 1: GitHub Actions CI/CD Pipeline

This project demonstrates how to set up a CI/CD pipeline using GitHub Actions to automate the deployment of a Node.js web application. The pipeline builds, tests, and deploys the application as a Docker image to DockerHub.

### Steps to Achieve the Task

#### 1. **Application Setup**
   - A simple Node.js application was created using Express.
   - The application has a single endpoint (`GET /`) that returns `Hello, World!`.

#### 2. **Testing**
   - A test suite was added using Jest and Supertest.
   - The test verifies that the `GET /` endpoint returns the expected response.
   - To run the tests locally, use the command:
     ```bash
     npm test
     ```

#### 3. **Dockerfile Creation**
   - A `Dockerfile` was created to containerize the Node.js application.
   - The `Dockerfile` specifies:
     - The base image (`node:16`).
     - The working directory (`/app`).
     - Installation of dependencies using `npm install`.
     - Copying the application code.
     - Exposing port `3000` for the app.
     - Starting the app with `npm start`.

#### 4. **GitHub Actions Workflow**
   - A GitHub Actions workflow file (`.github/workflows/main.yml`) was created to define the CI/CD pipeline.
   - The pipeline is triggered on every push to the `main` branch.

#### 5. **Pipeline Configuration**
   - The pipeline consists of the following steps:
     1. **Checkout Code**: Uses the `actions/checkout@v3` action to fetch the repository code.
     2. **Set Up Node.js**: Uses the `actions/setup-node@v3` action to set up Node.js version `16`.
     3. **Install Dependencies and Run Tests**: Installs dependencies using `npm install` and runs tests using `npm test`.
     4. **Log in to DockerHub**: Uses the `docker/login-action@v2` action to authenticate with DockerHub using credentials stored in GitHub Secrets (`DOCKER_USERNAME` and `DOCKER_PASSWORD`).
     5. **Build and Push Docker Image**: Builds a Docker image using the `docker build` command and pushes it to DockerHub using the `docker push` command.

#### 6. **GitHub Secrets**
   - DockerHub credentials were stored as GitHub Secrets:
     - `DOCKER_USERNAME`: Your DockerHub username.
     - `DOCKER_PASSWORD`: Your DockerHub password.

#### 7. **Deployment**
   - The Docker image is built and tagged as `<DOCKER_USERNAME>/node-app:latest`.
   - The image is pushed to DockerHub, making it available for deployment.

---

## Task 2: Jenkins CI/CD Pipeline

This section explains how to set up a Jenkins pipeline to automate the process of building, testing, and deploying the Node.js application.

### 1. **Running Jenkins on an Azure VM**
   - Launch an Cloud Virtual Machine (VM) with the following specifications:
     - OS: Ubuntu 24.04 LTS
     - Minimum 2 vCPUs and 4 GB RAM
   - SSH into the VM and install Docker:
     ```bash
     sudo apt update
     sudo apt install -y docker.io
     sudo systemctl start docker
     sudo systemctl enable docker
     ```
   - Run Jenkins in a Docker container:
     ```bash
     sudo docker run -d --name jenkins -p 8080:8080 -p 50000:50000 \
       -v jenkins_home:/var/jenkins_home \
       -v /var/run/docker.sock:/var/run/docker.sock \
       jenkins/jenkins:lts
     ```
   - Access Jenkins at `http://<VM_PUBLIC_IP>:8080`.
   - Follow the on-screen instructions to unlock Jenkins using the initial admin password:
     ```bash
     sudo docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
     ```
   - Install the recommended plugins during the setup process.

   - Install `npm` and `docker` inside the Jenkins container:
     ```bash
     sudo docker exec -it -u 0 jenkins bash
     apt-get update
     apt-get install -y npm docker.io
     usermod -aG docker jenkins
     exit
     ```

   ![Jenkins Dashboard](misc/a.png)

### 2. **Install Required Plugins**
   - Go to `Manage Jenkins` > `Manage Plugins`.
   - Install the following plugins:
     - `Pipeline`
     - `Docker Pipeline`
     - `GitHub Integration`

   ![Installed Plugins](misc/b.png)

### 3. **Configure DockerHub Credentials**
   - Go to `Manage Jenkins` > `Manage Credentials`.
   - Add two credentials:
     1. **DockerHub Username**:
        - ID: `docker-username`
        - Your DockerHub username
     2. **DockerHub Password**:
        - ID: `docker-password`
        - Your DockerHub password

### 4. **Add Jenkinsfile to Repository**
   - Add the `Jenkinsfile` to the root of your project repository. The `Jenkinsfile` defines the pipeline stages for building, testing, and deploying the application.

### 5. **Set Up a Jenkins Pipeline Job**
   - Create a new pipeline job in Jenkins:
     1. Go to `New Item` > Enter a name > Select `Pipeline`.
     2. Under `Pipeline` > `Definition`, select `Pipeline script from SCM`.
     3. Configure the repository URL and branch containing the `Jenkinsfile`.

   ![Pipeline Job Configuration](misc/c.png)

### 6. **Configure Jenkins to Detect GitHub Changes**
   - Go to your GitHub repository and navigate to `Settings` > `Webhooks`.
   - Add a new webhook with the following details:
     - **Payload URL**: `http://<VM_PUBLIC_IP>:8080/github-webhook/`
     - **Content type**: `application/json`
     - **Events**: Select "Just the push event."
   - Save the webhook.

   ![GitHub Webhook Configuration](misc/d.png)

### 7. **Pipeline Stages**
   - The pipeline consists of the following stages:
     1. **Checkout Code**: Pulls the latest code from the repository.
     2. **Install Dependencies and Test**: Installs dependencies using `npm install` and runs tests using `npm test`.
     3. **Build Docker Image**: Builds a Docker image for the application using the `Dockerfile`.
     4. **Push Docker Image**: Logs in to DockerHub and pushes the Docker image.

### 8. **Test the Pipeline**
   - Push changes to the repository to trigger the pipeline.
   - Monitor the Jenkins dashboard to ensure the pipeline runs successfully.

   ![Successful Pipeline Run](misc/e.png)

---

## File Structure
```
/Users/arzan03/elevate/task1/
├── app.js
├── server.js
├── test/
│   └── app.test.js
├── Dockerfile
├── .github/
│   └── workflows/
│       └── main.yml
├── Jenkinsfile
├── package.json
└── README.md
```

---

## Conclusion
This project demonstrates two CI/CD pipelines:
1. **GitHub Actions**: Automates testing, building, and deploying the application to DockerHub.
2. **Jenkins**: Provides an alternative CI/CD pipeline with similar functionality, configured using a `Jenkinsfile`.

Both pipelines ensure a streamlined process for rapid and reliable deployments.
