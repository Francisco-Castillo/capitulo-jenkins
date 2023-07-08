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
                            sh 'sudo rm -rf /var/lib/jenkins/workspace/package'
                        }
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

		stage ('Stage 3') {
			steps {
				sh 'curl -X POST https://api.telegram.org/bot$token/sendMessage -d chat_id=$chatId -d text="Build successfully"'
			}
			
		}
		
	}
}