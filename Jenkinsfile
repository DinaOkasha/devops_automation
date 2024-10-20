pipeline{
    
    agent any 
    tools {
        maven 'jenkins-maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/DinaOkasha/devops_automation.git']])
                
                sh 'mvn clean install'
                
            }
        }
        
        stage('Build Docker image'){
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
        
       stage('Deploy using k8s'){
            steps{
                
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml', kubeconfigId: 'k8s_deploy')
                    
                      }
           
                }
                    
                    
           }
    }
}
