pipeline {
    triggers { pollSCM('* * * * *') }
    agent {docker}

    stages {
        stage('Checkout') {
            steps {
                // Clone the private GitHub repository
                git credentialsId: 'githubloginssh', url: 'git@github.com:ahmedd-mahmoudd/Instabug.git'
            }
        }
        
        stage('Docker Info') {
            steps {
                // Run 'docker info' command to make sure the Docker daemon is running and jenkins user has access to docker.sock
                sh 'docker info'
            }
        }

        stage('Build') {
            steps {
                // Build the Docker image using the Dockerfile
                script {
                    def imageName = "xahmedmahmoudx/instabug:latest"
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
                    def imageName = "xahmedmahmoudx/instabug:latest"
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
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

