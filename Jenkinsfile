pipeline {
    agent {
        label 'vmagent'
    }
    
    environment {
        // Repository URLs
        APP_REPO_URL = 'git@github.com:yourorg/your-application.git'
        MANIFESTS_REPO_URL = 'git@github.com:yourorg/kubernetes-manifests.git'
        
        // Docker configuration
        DOCKER_REGISTRY = 'harbor.pdslkenya.com:80'
        DOCKER_PROJECT = 'pdsl'
        DOCKER_IMAGE_NAME = 'web-app'
        FULL_IMAGE_NAME = "${DOCKER_REGISTRY}/${DOCKER_PROJECT}/${DOCKER_IMAGE_NAME}"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        
        // Git configuration
        GIT_SSH_CREDENTIALS_ID = '20d5ba58-bee3-4658-afb1-94ed0bfb00ee'
        DOCKER_REGISTRY_CREDENTIALS_ID = 'harbor-credentials'
        
        // Deployment configuration
        MANIFESTS_BRANCH = 'main'
        HELM_CHART_PATH = 'charts/your-app'
        VALUES_FILE = 'charts/your-app/values.yaml'
        
        // Application-specific
        APP_NAME = 'web-app'
    }
    
    stages {
        stage('Checkout Application Code') {
            steps {
                script {        
                    // Get short commit hash for tagging
                //    env.GIT_COMMIT_SHORT = sh(
                //        script: 'git rev-parse --short HEAD',
                //        returnStdout: true
                //    ).trim()
                    env.IMAGE_TAG = "${env.BUILD_NUMBER}"
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Check for Dockerfile
                    if (!fileExists('Dockerfile')) {
                        error("Dockerfile not found in repository root")
                    }
                    
                    // Build Docker image
                    docker.build("${FULL_IMAGE_NAME}:${IMAGE_TAG}")
                    
                    // Also tag as latest for internal reference
                    //sh "docker tag ${FULL_IMAGE_NAME}:${IMAGE_TAG} ${FULL_IMAGE_NAME}:latest"
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    // Run tests inside the built container
                    docker.image("${FULL_IMAGE_NAME}:${IMAGE_TAG}").inside {
                        sh '''
                            echo "Running tests..."
                            # Add your test commands here, e.g.:
                            # npm test
                            # pytest
                            # go test ./...
                            # mvn test
                            echo "Tests completed successfully"
                        '''
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    // Login to Docker registry
                //    withCredentials([
                //        usernamePassword(
                //            credentialsId: "${DOCKER_REGISTRY_CREDENTIALS_ID}",
                //            usernameVariable: 'DOCKER_USER',
                //            passwordVariable: 'DOCKER_PASSWORD'
                //        )
                //    ]) {
                //        sh """
                //            echo "${DOCKER_PASSWORD}" | docker login ${DOCKER_REGISTRY} \
                //                --username "${DOCKER_USER}" \
                //                --password-stdin
                //        """
                //    }
                    
                    // Push the tagged image
                    sh "docker push ${FULL_IMAGE_NAME}:${IMAGE_TAG}"
                    
                    echo "Image pushed: ${FULL_IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }
}
