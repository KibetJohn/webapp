pipeline {
    agent {
        label 'main-agent' // Ensure this matches the label of your Docker in Docker agent
    }
    stages {
        stage('Build') {
            steps {
                checkout scmGit(
                         branches: [[name: 'main']],
                         userRemoteConfigs: [[url: 'https://github.com/KibetJohn/webapp.git']])
                script {
                    docker.image('maven:3.6.3-jdk-11').inside {
                        sh 'mvn clean install'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.image('maven:3.6.3-jdk-11').inside {
                        sh 'mvn test'
                    }
                }
            }
        }
    }
}
