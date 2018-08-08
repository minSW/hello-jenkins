pipeline {
	agent { node 'slave-2' }

	options {
		timeout(time: 40, unit: 'MINUTES' )
		timestamps()
	}

	stages {
		stage('Init') {
			steps {
				sh "git clone https://github.com/sktelecom-oslab/taco-scripts.git"
			}
		}

		stage('Run the script') {
			steps {
				sh """cd taco-scripts
				./010-init-env.sh
				"""
			}
		}
	}
	post {
		//always {
		//	..
		//}
		success {
			//notifyCompleted(true)
			slackSend (
				color: '#ACFA58',
				message: "COMPLETED: job '${env.JOB_NAME} [${env.BUILD_NUMBER}]] (${env.BUILD_URL})"
				)
		}
		failure {
			//notifyCompleted(false)
			slackSend (
				color: '#FA5882',
				message: "FAILED: job '${env.JOB_NAME} [${env.BUILD_NUMBER}]] (${env.BUILD_URL})"
				)
		}
	}
}