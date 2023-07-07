pipeline{
	agent any
        environment {
            token = credentials('token')
            chatId = credentials('chatId')
        }
	stages {
		stage ('Build') {
			steps {
				sh 'mvn clean package'
			}
                        post {
                            success {
                                echo 'Now Archiving...'
                                archieveArtifacts artifacts: '**/target/*.war'
                            }
                        }
		}

		stage ('Stage 2') {
			steps {
				echo 'Stage 2'
			}
			
		}

		stage ('Stage 3') {
			steps {
				sh 'curl -X POST https://api.telegram.org/bot$token/sendMessage -d chat_id=$chatId -d text="Build successfully"'
			}
			
		}
		
	}
}