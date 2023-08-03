pipeline {
    agent any
    tools {
        maven 'maven3.9.3'
    }
    stages{
        //checkout src code from github
        stage('scmcheckout'){
            steps{
                git branch: 'main', credentialsId: 'gitgub_jenkins', url: 'https://github.com/DevOpsproject-asn/springboot_mongo_project.git'
            }
        }
        stage('build the code'){
            steps{
                sh "mvn clean package"
            }
        }
        stage('build the docker image'){
            steps{
                sh "docker build -t cloudocker123456/springboot_mongo_project1 ."
            }
        }
        stage('dockerhub login&push the image'){
            steps{
                withCredentials([string(credentialsId: 'dockerHUB_cred', variable: 'dockerHub')]) {
                 sh "docker login -u cloudocker123456 -p ${dockerHub}"
            }
                 sh "docker push cloudocker123456/springboot_mongo_project1:latest"
        }
        stage('kubernates pod creation on cluster'){
            steps{
                sh "kubectl apply -f springbook.yaml"
                sh "kubectl apply -f mongo.yaml"
            }
        }
    }
}