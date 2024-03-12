pipeline {
  agent any
  
   tools {nodejs "node"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/ananys23/Deploy-NodeApp-to-AWS-EKS-using-Jenkins-Pipeline']])
                }
            }
        }
     
    stage('Node JS Build') {
      steps {
        sh 'npm install'
      }
    }
  
     stage('Build Node JS Docker Image') {
            steps {
                script {
                  sh 'docker build -t devopshint/node-app-1.0 .'
                }
            }
        }


        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'devopshintdocker', variable: 'devopshintdocker')]) {
                    sh 'docker login -u anany47 -p ${devopshintdocker}'
            }
            sh 'docker push devopshint/node-app-1.0'
        }
            }   
        }
         
     stage('Deploying Node App to Kubernetes') {
      steps {
        script {
          sh ('aws eks update-kubeconfig --name pc-eks --region ap-southeast-1')
          sh "kubectl get ns"
          sh "kubectl apply -f nodejsapp.yaml"
        }
      }
    }

  }
}
