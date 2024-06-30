pipeline {
    agent any

    environment {
        // Define environment variables if needed
        REPO1 = 'https://github.com/Chinmay998/2024q1.git'
        REPO2 = 'https://github.com/Chinmay998/2024-q2.git'
        DOCKER_IMAGE = 'server'
        PORT1 = '8080'
        PORT2 = '80'
    }

    stages {
        stage('Checkout Repositories') {
            parallel {
                stage('Checkout Repo1') {
                    steps {
                        git url: env.REPO1, branch: 'main'
                    }
                }
                stage('Checkout Repo2') {
                    steps {
                        git url: env.REPO2, branch: 'main'
                    }
                }
            }
        }
        
        stage('Build and Deploy Docker Containers') {
            steps {
                script {
                    def container_id = sh(script: "docker ps --filter 'status=running' --format '{{.ID}}'", returnStdout: true).trim()
                    if (container_id) {
                        sh "docker cp \$(pwd)/repo1/. \$(pwd)/repo2/. ${container_id}:/usr/share/nginx/html"
                    } else {
                        sh """
                            docker build -t ${DOCKER_IMAGE} .
                            docker run -d -p ${PORT1}:${PORT2} ${DOCKER_IMAGE}
                        """
                    }
                }
            }
        }
    }
}
