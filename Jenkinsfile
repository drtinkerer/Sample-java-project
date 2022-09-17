pipeline {
    agent any
    stages {
        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.build "drtinkerer/maventest:$BUILD_NUMBER"
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
         always {
             script {
                 slackSend channel: 'bhushan-personal-testing', 
                           color : 'good',
                           teamDomain: 'cldcvr',
                           message: "started ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>) $BRANCH_NAME",
                           tokenCredentialId: 'jenkins-ci-slack-token'
             }
         }  
     }
}
