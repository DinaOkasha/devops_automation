pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/DinaOkasha/devops_automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t drdinaokasha/devops_automation .'
                }
            }
        }
        
       stage('Push Docker image'){
            steps{
                
                script{
                    withCredentials([string(credentialsId: 'DOCKERHUB-PWD', variable: 'DOCKERHUBDB')]) {
                    
                    sh 'docker login -u drdinaokasha -p ${DOCKERHUBDB}'
                    
                    sh 'docker push drdinaokasha/devops_automation'
                    }
                }
                
                
            }
        } 
       
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}