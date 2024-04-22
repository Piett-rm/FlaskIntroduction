pipeline {
    agent any

    stages {
        stage('Connect AWS') {
            steps {
                withCredentials([usernamePassword(credentialsId: '1', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    script {
                        sh 'echo \${PASSWORD} | docker login --username AWS --password-stdin \${USERNAME}.dkr.ecr.us-east-1.amazonaws.com'
                    }
                }
            }
        }
        stage('Docker build and publish') {
			steps {
			    withCredentials([usernamePassword(credentialsId: '1', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
					sh '''
						docker build -t tp-integration:latest .
						docker tag tp-integration:latest \${USERNAME}.dkr.ecr.us-east-1.amazonaws.com/tp-integration:latest
						docker push \${USERNAME}.dkr.ecr.us-east-1.amazonaws.com/tp-integration:latest
					'''
				}
			}
		}
    }

    post {
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
