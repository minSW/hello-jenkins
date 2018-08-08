pipeline {
	agent { node 'slave-2' }

	options {
		timeout(time: 100, unit: 'MINUTES' )
		timestamps()
	}

	stages {
		stage('Init') {
			steps {
				sh """sudo -i
				git clone https://github.com/sktelecom-oslab/taco-scripts.git
				"""
			}
		}

		stage('Run the script 010-30') {
			steps {
			    dir('taco-scripts') {
			    	sh "sudo ./010-init-env.sh"
			    	sh """sudo sh -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
			    	sudo ./020-install-k8s.sh
			    	"""
			    	sh "sudo ./030-install-armada.sh"
			    }
			}
		}
	}
	post {
		success {
			//notifyCompleted(true)
			slackSend (
				color: '#ACFA58',
				message: "COMPLETED: job '${env.JOB_NAME} [${env.BUILD_NUMBER}]] (${env.BUILD_URL})"
				)
		}
		failure {
			//notifyCompleted(false)
			//dir('taco-scripts') {
			//	sh """rm -rf taco-scripts/
			//	sudo ./099-cleanup-all.sh
			//	"""
			//}
			slackSend (
				color: '#FA5882',
				message: "FAILED: job '${env.JOB_NAME} [${env.BUILD_NUMBER}]] (${env.BUILD_URL})"
				)
		}
	}
}