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
           sh """curl -X POST --data '{"attachments":[{"color":"#2CCA82","blocks":[{"type":"section","text":{"type":"mrkdwn","text":"Jenkins job `${env.JOB_NAME}` started"},"accessory":{"type":"button","text":{"type":"plain_text","text":"Click to view on Jenkins UI ","emoji":true},"value":"${env.BUILD_URL}","action_id":"button-action"}},{"type":"section","fields":[{"type":"mrkdwn","text":"*Job Name:*\n`${env.JOB_NAME}`"},{"type":"mrkdwn","text":"*Build Number:*\n`${env.BUILD_NUMBER}`"},{"type":"mrkdwn","text":"*Build Result:*\n`${currentBuild.result}`"},{"type":"mrkdwn","text":"*Build Duration:*\n`${currentBuild.durationString}`"}]}]}]}' https://hooks.slack.com/services/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"""
         }  

         failure {
           sh """curl -X POST --data '{"attachments":[{"color":"#CA2C5E","blocks":[{"type":"section","text":{"type":"mrkdwn","text":"Jenkins job `${env.JOB_NAME}` started"},"accessory":{"type":"button","text":{"type":"plain_text","text":"Click to view on Jenkins UI ","emoji":true},"value":"${env.BUILD_URL}","action_id":"button-action"}},{"type":"section","fields":[{"type":"mrkdwn","text":"*Job Name:*\n`${env.JOB_NAME}`"},{"type":"mrkdwn","text":"*Build Number:*\n`${env.BUILD_NUMBER}`"},{"type":"mrkdwn","text":"*Build Result:*\n`${currentBuild.result}`"},{"type":"mrkdwn","text":"*Build Duration:*\n`${currentBuild.durationString}`"}]}]}]}' https://hooks.slack.com/services/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"""
         }  
     }
}
