pipeline {
    agent {
        label 'vmagent' // Ensure this matches the label of your Docker in Docker agent
    }
    stages {
        stage('Build') {
            steps {
                //retry(2) {
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
                    docker.image('harbor.pdslkenya.com:80/pdsl/nginx:latest').inside {
                        sh 'ls'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.image('harbor.pdslkenya.com:80/pdsl/nginx:latest').inside {
                        sh 'ls'
                    }
                }
            }
        }
    }
}
