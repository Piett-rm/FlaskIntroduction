pipeline {
    agent any

    stages {
        stage('Connect AWS') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'my_credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                        echo "Username: \${USERNAME}"
                        echo "Password: \${PASSWORD}"

                    """
                    script {
                        sh 'echo \${PASSWORD} | docker login --username AWS --password-stdin \${USERNAME}.dkr.ecr.us-east-1.amazonaws.com'
                    }
                }
            }
        }
        stage('Docker build and publish') {
			steps {
				dir('./AppRemi') {
					sh '''
						docker build -t tp-integration:latest .
						docker tag tp-integration:latest 065334477167.dkr.ecr.us-east-1.amazonaws.com/tp-integration:latest
						docker push 065334477167.dkr.ecr.us-east-1.amazonaws.com/tp-integration:latest
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
