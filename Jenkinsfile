pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'http://34.47.134.214:9000'
        SONARCUBE_TOKEN = credentials('sonarqube')

        DOCKER_IMAGE = "payment-app_frontend"
        DOCKER_IMAGE_1 = "payment_service-backend"
        DOCKER_IMAGE_2 = "auth_service-backend"

        NEXUS_REPO_URL = '34.47.183.188:8085/repository/payment-app/'
        NEXUS_CREDENTIALS_ID = 'nexuscred' 
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/basilbaiju/payment-app.git'
            }
        }

        /*stage('SonarQube Scanning') {
            steps {
                script {
                    withSonarQubeEnv('sonar-scanner') { 
                        sh """
                        ${tool('sonar-scanner')}/bin/sonar-scanner \
                            -Dsonar.projectKey=payment-app \
                            -Dsonar.host.url=${SONARQUBE_URL} \
                            -Dsonar.login=${SONARCUBE_TOKEN}
                        """
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the frontend Docker image
                    sh 'docker build -t $DOCKER_IMAGE ./frontend'
                    sh 'docker build -t $DOCKER_IMAGE_1 ./backend/payment_service'
                    sh 'docker build -t $DOCKER_IMAGE_2 ./backend/auth_service'

                    sh 'docker tag $DOCKER_IMAGE $NEXUS_REPO_URL$DOCKER_IMAGE:$BUILD_NUMBER'
                    sh 'docker tag $DOCKER_IMAGE $NEXUS_REPO_URL$DOCKER_IMAGE:latest'

                    sh 'docker tag $DOCKER_IMAGE_1 $NEXUS_REPO_URL$DOCKER_IMAGE_1:$BUILD_NUMBER'
                    sh 'docker tag $DOCKER_IMAGE_1 $NEXUS_REPO_URL$DOCKER_IMAGE_1:latest'

                    sh 'docker tag $DOCKER_IMAGE_2 $NEXUS_REPO_URL$DOCKER_IMAGE_2:$BUILD_NUMBER'
                    sh 'docker tag $DOCKER_IMAGE_2 $NEXUS_REPO_URL$DOCKER_IMAGE_2:latest'
                }
            }
        }

         stage('Push Docker Image to Nexus') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: "$NEXUS_CREDENTIALS_ID", 
                        usernameVariable: 'NEXUS_USERNAME', 
                        passwordVariable: 'NEXUS_PASSWORD'
                    )]) {
                        // Log in to Nexus Docker registry
                        sh '''
                        echo $NEXUS_PASSWORD | docker login http://$NEXUS_REPO_URL \
                        -u $NEXUS_USERNAME --password-stdin
                        '''

                        // Push the Frontend Docker image with the build number tag
                        sh 'docker push $NEXUS_REPO_URL$DOCKER_IMAGE:$BUILD_NUMBER'
                        sh 'docker push $NEXUS_REPO_URL$DOCKER_IMAGE:latest'

                         // Push the Payment-service Docker image with the build number tag
                        sh 'docker push $NEXUS_REPO_URL$DOCKER_IMAGE_1:$BUILD_NUMBER'
                        sh 'docker push $NEXUS_REPO_URL$DOCKER_IMAGE_1:latest'

                        // Push the Auth-service Docker image with the build number tag
                        sh 'docker push $NEXUS_REPO_URL$DOCKER_IMAGE_2:$BUILD_NUMBER'
                        sh 'docker push $NEXUS_REPO_URL$DOCKER_IMAGE_2:latest'

                    }
                }
            }
        }*/



        stage('Deploy Logging Stack') {
            steps {
                sh '''
                cd logging
                docker-compose down || true
                docker-compose up -d
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                docker-compose down || true
                docker-compose up -d
                '''
            }
        }

        stage('Deploy Monitoring Stack') {
            steps {
                sh '''
                cd monitoring
                docker-compose down || true
                docker-compose up -d
                '''
            }
        }
    }

    post {
        success {
            echo "Application successfully deployed and running!"
        }
        failure {
            echo "Deployment failed."
        }
    }
}
  
