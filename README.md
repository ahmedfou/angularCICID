# Angular CI/CD Pipeline with Jenkins

This repository demonstrates how to set up a **Continuous Integration and Continuous Deployment (CI/CD)** pipeline for an Angular application using **Jenkins**. The pipeline includes steps for linting, building, testing, and deploying the Angular application.

---

## Prerequisites

Before setting up the pipeline, ensure the following are installed and configured:

1. **Jenkins**: Installed and running.
2. **Node.js**: Installed on the Jenkins server.
3. **Angular CLI**: Installed globally on the Jenkins server.
4. **Git**: A Git repository containing your Angular project.

---

## Steps to Set Up the Pipeline

### 1. Install Required Jenkins Plugins
Install the following Jenkins plugins:
- **NodeJS Plugin**: To manage Node.js installations.
- **Pipeline**: To create and manage Jenkins pipelines.
- **Git Plugin**: To integrate with Git repositories.

### 2. Configure Node.js in Jenkins
1. Go to **Manage Jenkins** > **Global Tool Configuration**.
2. Add a new Node.js installation:
   - Name: `NodeJS` (or any name you prefer).
   - Version: Choose a stable LTS version (e.g., 18.x or 20.x).

### 3. Create a Jenkins Pipeline
1. Go to the Jenkins dashboard and click on **New Item**.
2. Enter a name for your pipeline (e.g., `Angular-CI-CD`).
3. Select **Pipeline** and click **OK**.

### 4. Configure the Pipeline
1. In the pipeline configuration page, scroll down to the **Pipeline** section.
2. In the **Definition** dropdown, select **Pipeline script from SCM**.
3. Choose **Git** as the SCM.
4. Enter the repository URL and credentials if needed.
5. In the **Script Path**, enter the path to your `Jenkinsfile` (e.g., `Jenkinsfile`).
6. Save the configuration.

---

## Jenkinsfile

Create a `Jenkinsfile` in the root of your Angular project. This file defines the pipeline stages.

```groovy
pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // Name of the Node.js installation configured in Jenkins
    }

    environment {
        ANGULAR_CLI = 'ng'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/your-angular-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build --prod'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                // Add your deployment steps here
                // For example, deploying to a server or cloud platform
                sh 'echo "Deploying to production..."'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
