pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Abhijan2023/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t abhishekp006/kubernetes:$BUILD_NUMBER .'
                }
            }
        }
        stage('Push image to hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u abhishekp006 -p ${dockerhubpwd}'
                        
                    }
                    sh 'docker push abhishekp006/kubernetes:$BUILD_NUMBER'
                }
            }
        }
       stage('Deploy to Kubernetes') {
            steps {
                script {
                    def kubeconfigPath = '/path/to/kubeconfig'
                    def deploymentConfigPath = '/path/to/deploymentservice.yaml'
                    
                    sh "kubectl --kubeconfig=${kubeconfigPath} apply -f ${deploymentConfigPath}"
                    sh "kubectl --kubeconfig=${kubeconfigPath} autoscale deployment Deployment --cpu-percent=80 --min=1 --max=10"              
                }
            }
        }
    }    
}
