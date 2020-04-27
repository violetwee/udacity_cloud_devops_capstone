pipeline {
	agent any
	stages {

		stage('Lint HTML') {
			steps {
				sh 'tidy -q -e ./blue/*.html'
				sh 'tidy -q -e ./green/*.html'
			}
		}
		
		stage('Build Docker images') {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'DOCKER_CRED', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
						cd blue
						docker build --tag=blue .
					'''
				}
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'DOCKER_CRED', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
						cd green
						docker build --tag green .
					'''
				}
			}
		}

		stage('Push Docker image To Docker Hub') {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'DOCKER_CRED', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]){
					sh '''
						cd blue
						docker login -u $USERNAME -p $PASSWORD
						docker image tag blue violetwee/blue
						docker image push violetwee/blue
					'''
				}
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'DOCKER_CRED', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]){
					sh '''
						cd green
						docker login -u $USERNAME -p $PASSWORD
						docker image tag green violetwee/green
						docker image push violetwee/green
					'''
				}
			}
		}    

		stage('Set cluster as current context') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws_cred_capstone') {
					sh '''
						aws eks update-kubeconfig --name capstone_cluster
					'''
				}
			}
		}

		stage('Deploy blue container') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws_cred_capstone') {
					sh '''
						kubectl apply -f ./blue/blue-controller.json
					'''
				}
			}
		}

		stage('Deploy green container') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws_cred_capstone') {
					sh '''
						kubectl apply -f ./green/green-controller.json
					'''
				}
			}
		}

		stage('Create the service in the cluster, redirect to blue') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws_cred_capstone') {
					sh '''
						kubectl apply -f ./blue/blue-service.json
					'''
				}
			}
		}

		stage('Redirect traffic confirmation') {
        steps {
            input "Do you want to redirect all traffic to GREEN?"
        }
    }

		stage('Create the service in the cluster, redirect to green') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws_cred_capstone') {
					sh '''
						kubectl apply -f ./green/green-service.json
					'''
				}
			}
		}

	}
}
