pipeline{

    agent any
     environment{
        registry = "131087090100.dkr.ecr.eu-north-1.amazonaws.com/springboot123"
    }
    stages{
     stage('Cloning the Code') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/dinu2907/springboot-app.git']]])     
            }
   }
    stage('Building the code'){
        steps{
            sh 'mvn clean install'
        }
        
            }
stage('Building Docker image') {
      steps{
        script {
          dockerImage = docker.build registry 
        }
      }
    }
     stage('Push into ECR Repo'){
        steps{
            sh 'aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin 131087090100.dkr.ecr.eu-north-1.amazonaws.com'
            sh 'docker push 131087090100.dkr.ecr.eu-north-1.amazonaws.com/springboot123:latest'
        }
    }
    
    stage('Deploy to K8s Cluster'){
        steps{
   withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', serverUrl: '') {
    // some block
    sh "kubectl delete all --all"
    sh "kubectl apply -f eks-deploy-k8s.yaml"
    }
 }
}

        

 
  }
}
