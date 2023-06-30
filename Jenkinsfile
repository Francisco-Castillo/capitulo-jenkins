pipeline{
	agent any
	stages {
		stage ('Stage 1') {
			steps {
				echo 'Stage 1'
			}

		}

		stage ('Stage 2') {
			steps {
				echo 'Stage 2'
			}
			
		}

		stage ('Stage 3') {
			steps {
				sh `curl -X POST https://api.telegram.org/bot${token}/sendMessage -d chat_id=${chatId} -d text="Build successfully"`
			}
			
		}
		
	}
}