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
           withCredentials([string(credentialsId: 'slack-webhook', variable: 'WEBHOOK')]) {
                 sh """curl -X POST --data '{"attachments":[{"color":"#2CCA82","blocks":[{"type":"section","text":{"type":"mrkdwn","text":"*Jenkins job finished building !*"},"accessory":{"type":"button","text":{"type":"plain_text","text":"Click to view on Jenkins UI ","emoji":true},"url":"${env.BUILD_URL}","style":"primary","action_id":"button-action"}},{"type":"section","fields":[{"type":"mrkdwn","text":"*Job Name:*\n`${env.JOB_NAME}`"},{"type":"mrkdwn","text":"*Build Number:*\n`${env.BUILD_NUMBER}`"},{"type":"mrkdwn","text":"*Build Result:*\n`${currentBuild.result}`"},{"type":"mrkdwn","text":"*Build Duration:*\n`${currentBuild.durationString}`"}]}]}]}' ${WEBHOOK}"""
            }
         }  

         failure {
           withCredentials([string(credentialsId: 'slack-webhook', variable: 'WEBHOOK')]) {
                 sh """curl -X POST --data '{"attachments":[{"color":"#CA2C5E","blocks":[{"type":"section","text":{"type":"mrkdwn","text":"*Jenkins job finished building !*"},"accessory":{"type":"button","text":{"type":"plain_text","text":"Click to view on Jenkins UI ","emoji":true},"url":"${env.BUILD_URL}","style":"danger","action_id":"button-action"}},{"type":"section","fields":[{"type":"mrkdwn","text":"*Job Name:*\n`${env.JOB_NAME}`"},{"type":"mrkdwn","text":"*Build Number:*\n`${env.BUILD_NUMBER}`"},{"type":"mrkdwn","text":"*Build Result:*\n`${currentBuild.result}`"},{"type":"mrkdwn","text":"*Build Duration:*\n`${currentBuild.durationString}`"}]}]}]}' "${WEBHOOK}" """
            }
         }  
     }
}
