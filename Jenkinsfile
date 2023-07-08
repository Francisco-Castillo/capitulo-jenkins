pipeline{
    agent any

    environment {
        token = credentials('token')
        chatId = credentials('chatId')
    }

    tools {
        maven 'maven'
    }

    stages {
        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deploy to staging') {
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage ('Notification') {
            steps {
                sh 'curl -X POST https://api.telegram.org/bot$token/sendMessage -d chat_id=$chatId -d text="Build successfully"'
            }
        }

        stage ('Deploy to production') {
            steps {
                timeout(time:5, unit: 'DAYS'){
                    input message: 'Aprobar pasaje a produccion?'
                }
                build job: 'deploy-to-production'
            }
            post {
                success {
                    echo 'Desplegado en produccion.'
                }
                failure {
                    echo 'Fallo el despliegue en produccion.'
                }
            }
        }
    }
}