pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clone the private GitHub repository
                git credentialsId: 'githublogin', url: 'https://github.com/ahmedd-mahmoudd/Instabug.git'
            }
        }
        
        stage('Build') {
            steps {
                // Build the Docker image using the Dockerfile
                script {
                    def imageName = "xahmedmahmoudx/instabug:${env.BUILD_NUMBER}"
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerlogin') {
                        sh "docker build -t ${imageName} ."
                    }
                }
            }
        }

        stage('Push') {
            steps {
                // Push the Docker image to Docker Hub
                script {
                    def imageName = "xahmedmahmoudx/instabug:${env.BUILD_NUMBER}"
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerlogin') {
                        sh "docker push ${imageName}"
                    }
                }
            }
        }
    }

    post {
        always {
            // Cleanup: Remove the Docker image from the build machine
            cleanWs()
        }
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}

// Trigger the pipeline every 1 minute to check for new changes
triggers {
    pollSCM('* * * * *')
}
