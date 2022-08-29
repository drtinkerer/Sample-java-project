pipeline {
    agent any
    stages {
        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.build "aaditya7789/maventest:$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy our image') {
            steps {
                script {
                    // Assume the Docker Hub registry by passing an empty string as the first parameter
                    docker.withRegistry('' , 'dockerhub') {
                        dockerImage.push()
                    }
                }
            }
        }
    }
    
     post {
         steps {
             script {
                 slackSend tokenCredentialId: 'slack-token'
             }
         }  
     }
}
