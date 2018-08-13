pipeline {
	agent { node 'slave-root' }

	options {
		timeout(time: 1000, unit: 'MINUTES' )
		timestamps()
	}

	stages {
/*
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
		

		stage('Run the script 040-41') {
			steps {
			    dir('taco-scripts') {
			    	sh "sudo ./040-deploy-openstack.sh"
			    	sh "sudo ./041-deploy-mon.sh"
			    }
			}
		}
		

		stage('Run the script 050 _ create os resources') {
			steps {
			    dir('taco-scripts') {
			    	sh "sudo ./050-create-os-resources.sh"
			    }
			}
		}
*/
		stage('Test os') {
			steps {
				dir('taco-scripts') {
					sh """ . adminrc
					kubectl get po --all-namespaces > pod_status.txt
					openstack service list > os_list.txt
					openstack network list >> os_list.txt
					openstack server list >> os_list.txt
					"""
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
			/*
			dir('taco-scripts') {
				sh """rm -rf taco-scripts/
				sudo ./099-cleanup-all.sh
				"""
			}
			*/
			slackSend (
				color: '#FA5882',
				message: "FAILED: job '${env.JOB_NAME} [${env.BUILD_NUMBER}]] (${env.BUILD_URL})"
				)
		}
	}
}