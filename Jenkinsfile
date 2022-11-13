pipeline {
	agent none
    stages {
        stage('Build') {
			agent {
				docker {
					image 'node:lts-buster-slim'
					args '-p 3000:3000'
				}
			}
			steps {
                sh 'npm install'
            }
        }
        stage('Test') {
			agent {
				docker {
					image 'node:lts-buster-slim'
					args '-p 3000:3000'
				}
			}
			steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
			agent {
				docker {
					image 'node:lts-buster-slim'
					args '-p 3000:3000'
				}
			}
			steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
		stage('OWASP DependencyCheck') {
			agent any
			steps {
				dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'Default'
			}	
		}
    }
}
