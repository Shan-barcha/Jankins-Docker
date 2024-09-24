pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "alishan001/cicd_pipeline"
        DOCKER_CREDENTIALS_ID = "Shan-Docker"  // For DockerHub
        GIT_CREDENTIALS_ID = "github-token"    // New GitHub PAT credential
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Use GitHub token for accessing the repository
                git credentialsId: "${GIT_CREDENTIALS_ID}", url: 'https://github.com/Shan-barcha/Jenkins-Docker.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install the project dependencies
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image from Dockerfile in the repo
                    dockerImage = docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Run Tests (Optional)') {
            steps {
                script {
                    // Run any tests inside the container
                    dockerImage.inside {
                        sh 'npm test' // Modify this based on your test script or remove if no tests
                    }
                }
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        // Push the built image with both build ID and 'latest' tags
                        dockerImage.push("${env.BUILD_ID}")
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    // Run the app container, exposing port 8081
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





// pipeline { 
  
//    agent any

//    stages {
   
//      stage('Install Dependencies') { 
//         steps { 
//            sh 'npm install' 
//         }
//      }
     
//      stage('Test') { 
//         steps { 
//            sh 'echo "testing application..."'
//         }
//       }

//          stage("Deploy application") { 
//          steps { 
//            sh 'echo "deploying application..."'
//          }

//      }
  
//    	}

//    }


// pipeline {
//   agent any
//   stages {
//     stage("checkout") {
//       steps {
//         checkout scm
//       }
//     }

//     stage("Test") {
//       steps {
//         sh 'sudo apt install npm'
//         sh 'npm test'
//       }
//     }

//     stage("Build") {
//       steps {
//         sh 'npm run build'
//       }
//     }

//     stage("Build Image") {
//      sh 'docker build -t my-node-app:1.0 .'
//     }
//   }

// }