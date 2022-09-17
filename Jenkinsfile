pipeline {
    agent any
    stages {
        stage('Building our image') {
            steps {
                script {
                    dockerImage = sdocker.build "drtinkerer/maventest:$BUILD_NUMBER"
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
                           teamDomain: 'cldcvr',
                           message: "Jenkins pipeline Job `${env.JOB_NAME}` started !",
                           tokenCredentialId: 'jenkins-ci-slack-token'
             }
         }  
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
