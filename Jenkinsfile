pipeline {
    agent any
    stages {
        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.jbuild "drtinkerer/maventest:$BUILD_NUMBER"
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
         success {
             script {
                 slackSend channel: 'bhushan-personal-testing', 
                           color : 'good',
                           teamDomain: 'cldcvr',
                           message: "Job Name : ${env.JOB_NAME} \n Build Number : ${env.BUILD_NUMBER} \n Build Result : ${currentBuild.result} \n Build Duration : ${currentBuild.durationString} \n (<${env.BUILD_URL}|View on Jenkins GUI>)",
                           tokenCredentialId: 'jenkins-ci-slack-token'
             }
         }  
         failure {
             script {
                 slackSend channel: 'bhushan-personal-testing', 
                           color : 'danger',
                           teamDomain: 'cldcvr',
                           message: "Job Name : ${env.JOB_NAME} \n Build Number : ${env.BUILD_NUMBER} \n Build Result : ${currentBuild.result} \n Build Duration : ${currentBuild.durationString} \n (<${env.BUILD_URL}|View on Jenkins GUI>)",
                           tokenCredentialId: 'jenkins-ci-slack-token'
             }
         }  
     }
}
