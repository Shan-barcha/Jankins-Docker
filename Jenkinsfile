pipeline {
    agent any

    tools {
        nodejs "node"  // Ensure NodeJS Plugin is installed and this "node" matches the configured NodeJS tool
    }

    environment {
        DOCKER_IMAGE = "alishan001/cicd_pipeline"
        DOCKER_CREDENTIALS_ID = "Shan-Docker"  // For DockerHub
        GIT_CREDENTIALS_ID = "github-token"    // The GitHub token credential ID
    }

    stages {
        stage('Cloning Git') {
            steps {
                // Use the correct branch (most likely "main" instead of "master")
                git url: 'https://github.com/Shan-barcha/Jankins-Docker.git',
                    credentialsId: "${GIT_CREDENTIALS_ID}",
                    branch: 'main' // Update this to "main" if your repository uses it
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install project dependencies using Node.js
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image from Dockerfile in the repository
                    dockerImage = docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image to DockerHub') {
             steps {
                 script {
                   docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                   dockerImage.push("${env.BUILD_ID}")
                   dockerImage.push('latest')
            }
        }
    }
}


        stage('Deploy Container') {
            steps {
                script {
                    // Deploy the container exposing port 8081
                    dockerImage.run('-p 8081:8081')
                }
            }
        }
    }

    post {
        always {
            // Clean up the workspace after build
            cleanWs()
        }
        success {
            echo 'Build, push, and deployment succeeded!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
