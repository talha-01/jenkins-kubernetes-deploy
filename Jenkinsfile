pipeline {
	agent { label "master" }
	environment {
		APP_REPO_NAME = "talhas/phonebook"
        APP_FILE = fileExists "/home/ubuntu/jenkins-kubernetes-deploy"
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
            steps { 
                script {
				    sshagent(credentials : ['talha-virginia']) {
                        sh 'ssh -t -t ubuntu@54.210.214.185 -o StrictHostKeyChecking=no "git clone https://github.com/talha-01/jenkins-kubernetes-deploy.git"'
                        sh 'ssh -t -t ubuntu@54.210.214.185 -o StrictHostKeyChecking=no "kubectl apply -f jenkins-kubernetes-deploy/kubernetes"'
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