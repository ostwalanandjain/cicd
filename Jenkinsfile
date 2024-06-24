pipeline {
    agent any

parameters {
  booleanParam defaultValue: true, description: 'false', name: 'Deploy_to_Prod'
}

    stages {
        stage('Checkout GitHub repo') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ostwalanandjain/helloDocker']])
            }
        }
        
        stage('Build and Tag Docker Image') {
            steps {
                script {
                    sh 'docker build . -t hellodocker'
                    sh 'docker tag hellodocker anandjain420/hellodocker'
                }
            }
        }
        
        stage('Push the Docker Image to DockerHUb') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker_hub', variable: 'docker_hub')]) {
                    sh 'docker login -u anandjain420 -p ${docker_hub}'
}
                    sh 'docker push anandjain420/hellodocker'
                }
            }
        }
        
        stage('Deploy deployment and service file') {
            steps {
                script {
                    kubernetesDeploy configs: 'deploymentsvc.yaml', kubeconfigId: 'kubernetes_config'
                }
            }
        }
    }
}
