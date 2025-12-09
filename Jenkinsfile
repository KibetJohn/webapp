pipeline {
    agent {
        label 'main-agent' // Ensure this matches the label of your Docker in Docker agent
    }
    stages {
        stage('Build') {
            steps {
                //retry(3) {
                //  timeout(time: 2, unit: 'MINUTES') {
                //  checkout([$class: 'GitSCM',
                //  branches: [[name: "main"]],
                //  doGenerateSubmoduleConfigurations: false,
                //  extensions: [],
                //  submoduleCfg: [],
                //  userRemoteConfigs: [[url: 'https://github.com/KibetJohn/webapp.git']]
                //  ])
                // }
                //}
                script {
                    docker.image('maven:3.6.3-jdk-11').inside {
                        sh 'ls'
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
