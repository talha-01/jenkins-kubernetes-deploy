pipeline {
	agent { label "master" }
	environment {
		APP_REPO_NAME = "talhas/phonebook"
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

        stage('Testing') {
            res = sh(script: "test -d ${target_dir} && echo '1' || echo '0' ", returnStdout: true).trim()
            if(res=='1'){
                echo 'yes'
            } else {
               echo 'no'
            }
        }
		stage('Renew deployment') {
			steps {
				script {
					sshagent(credentials : ['talha-virginia']) {
<<<<<<< HEAD
						sh 'ssh -t -t ubuntu@54.210.214.185 -o StrictHostKeyChecking=no "kubectl set image deployment/phonebook-deployment phonebook=talhas/phonebook:${BUILD_ID} --record"'
=======
						sh "echo pwd"
						IS_FIRST = sh 'ssh -t -t ubuntu@54.197.95.143 -o StrictHostKeyChecking=no "kubectl get pods"'
                        			if (IS_FIRST == 'No resources found in default namespace.') {
                                			sh 'ssh -t -t ubuntu@54.197.95.143 -o StrictHostKeyChecking=no "kubectl get svc"'
                        			}
>>>>>>> f03e3285b8714acf018ded9f30cea9cc838cd237
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
