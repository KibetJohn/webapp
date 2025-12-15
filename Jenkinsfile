@Library('pdsl-shared-libs@main') _
service_name="sms-auth-service"

pipeline {
    agent {
        label 'vmagent'
    }
    
    environment {
        // Repository URLs
        APP_REPO_URL = 'git@github.com:yourorg/your-application.git'
        MANIFESTS_REPO_URL = 'git@github.com:yourorg/kubernetes-manifests.git'
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
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Check for Dockerfile
                    if (!fileExists('Dockerfile')) {
                        error("Dockerfile not found in repository root")
                    }
                    
                     build_image("${service_name}", "${env.BUILD_NUMBER}", [ VAULT_PSWD: "main"] )
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    // Run tests inside the built container
                   // docker.image("${FULL_IMAGE_NAME}:${IMAGE_TAG}").inside {
                        sh '''
                            echo "Running tests..."
                            # Add your test commands here, e.g.:
                            # npm test
                            # pytest
                            # go test ./...
                            # mvn test
                            echo "Tests completed successfully"
                        '''
                    //}
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
                    push_image("${service_name}")
                }
            }
        }
    }
}
