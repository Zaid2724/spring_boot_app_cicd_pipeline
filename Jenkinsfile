pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3.9'
    }

    environment {
        APP_NAME     = "shopping-cart"
        IMAGE_NAME   = "zaidevs/shopping"
        IMAGE_TAG    = "${BUILD_NUMBER}"
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jaiswaladi246/Ekart.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube-Server') {
                    sh """
                    $SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectKey=shopping-cart \
                    -Dsonar.projectName=shopping-cart \
                    -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage('Trivy FS Scan') {
            steps {
                sh 'trivy fs --exit-code 1 --severity HIGH,CRITICAL .'
            }
        }

        stage('Docker Build & Trivy Image Scan') {
            steps {
                sh """
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} -f docker/Dockerfile .
                docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${IMAGE_NAME}:latest
                trivy image --exit-code 1 --severity HIGH,CRITICAL ${IMAGE_NAME}:latest
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry(credentialsId: 'docker-cred') {
                    sh """
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    docker push ${IMAGE_NAME}:latest
                    """
                }
            }
        }

        stage('Deploy Application') {
            steps {
                sh """
                docker rm -f ${APP_NAME} || true
                docker run -d --name ${APP_NAME} -p 8070:8070 ${IMAGE_NAME}:latest
                """
            }
        }

        stage('Health Check') {
            steps {
                sh 'sleep 10 && curl -f http://localhost:8070 || exit 1'
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs for details."
        }
    }
}

