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


pipeline {
  agent any
  stages {
    stage("checkout") {
      steps {
        checkout scm
      }
    }

    stage("Test") {
      steps {
        sh 'sudo apt install npm'
        sh 'npm test'
      }
    }

    stage("Build") {
      steps {
        sh 'npm run build'
      }
    }

    stage("Build Image") {
     sh 'docker build -t my-node-app:1.0 .'
    }
  }

}