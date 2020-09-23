pipeline {
	agent { label "master" }
	environment {
		APP_REPO_NAME = "talhas/phonebook"
        APP_FILE = fileExists "/home/ubuntu/jenkins-kubernetes-deply"
	}
	stages {
		stage('Build Docker Image') {
			steps {
				sh 'docker build -t phonebook:latest https://github.com/talha-01/jenkins-kubernetes-deploy.git'
				sh 'docker tag phonebook:latest $APP_REPO_NAME:latest'
				sh 'docker tag phonebook:latest $APP_REPO_NAME:${BUILD_ID}'
				sh 'docker images'
			}
		}
		stage('Push Image to Docker Hub') {
			steps {
				withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
				sh 'docker push $APP_REPO_NAME:latest'
				sh 'docker push $APP_REPO_NAME:${BUILD_ID}'
				}
			}
		}
        stage('Check the App File') {
            when { expression { APP_FILE == 'true' } }
            steps {
                echo "file exists"
            }
        }
        stage('Clone the Git File') {
            when { expression { APP_FILE == 'false' } }
            steps {
                git clone https://github.com/talha-01/jenkins-kubernetes-deploy.git
            }
        }
		stage('Renew deployment') {
			steps {
				script {
					sshagent(credentials : ['talha-virginia']) {
						sh 'ssh -t -t ubuntu@54.210.214.185 -o StrictHostKeyChecking=no "kubectl set image deployment/phonebook-deployment phonebook=talhas/phonebook:${BUILD_ID} --record"'
					}
				}
			}
		}
	}
 	post {
        	always {
            		echo 'Deleting all local images'
            	sh 'docker image prune -af'
        	}
	}
}