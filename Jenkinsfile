pipeline {
	agent any
	environment {
		PROJECT_ID = 'project my-project-2024-442302'
		CLUSTER_NAME = 'kube'
		LOCATION = 'asia-northeast3-a'
		CREDENTIALS_ID = 'gke'
	}
	stages {
		stage("Checkout code") {
			steps {
				checkout scm
			}
		}
		stage("Copy .env file") {
			steps {
				script {
					sh "cp /var/jenkins_home/.env ./code/.env"
				}
			}
		}
		stage("Build image") {
			steps {
				script {
					myapp = docker.build("kimgeunhye21/wimb:${env.BUILD_ID}")
				}
			}
		}
		stage("Push image") {
			steps {
				script {
					docker.withRegistry('https://registry.hub.docker.com', 'kimgeunhye21') {
						myapp.push("latest")
						myapp.push("${env.BUILD_ID}")
					}
				}
			}
 		}
		stage('Deploy to GKE') {
			when {
				branch 'main'
			}
			steps{
				sh "sed -i 's/wimb:latest/wimb:${env.BUILD_ID}/g' deployment.yaml"
				step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, 
verifyDeployments: true])
			}
		}
	}
}
