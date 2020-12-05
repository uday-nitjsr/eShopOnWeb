pipeline {
  agent any
  stages {
    stage('Docker Build') {
      steps {
        sh "sudo docker-compose build"
      }
    }
    stage('Docker delete existing containers') {
      steps {
        sh "sudo docker ps --filter 'label=name=Demo_App' -q | xargs --no-run-if-empty sudo docker container stop"
        sh "sudo docker ps --filter 'label=name=Demo_App' -q | xargs -r sudo docker container rm"
      }
    }
     stage('Docker containers up') {
      steps {
        sh "sudo docker-compose up -d"
      }
    }
    stage('Docker Push') {
      steps {
        echo "docker push"
       # withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
       #   sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
       #  sh "docker push kmlaydin/podinfo:${env.BUILD_NUMBER}"
        }
      }
    }
    stage('Docker Remove Image') {
      steps {
        echo "docker remove"
       # sh "docker rmi kmlaydin/podinfo:${env.BUILD_NUMBER}"
      }
    }
    stage('Apply Kubernetes Files') {
      steps {
          echo "kubernetes"
        #  withKubeConfig([credentialsId: 'kubeconfig']) {
         # sh 'cat deployment.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'
         # sh 'kubectl apply -f service.yaml'
        }
      }
  }
}
post {
    success {
      slackSend(message: "Pipeline is successfully completed.")
    }
    failure {
      slackSend(message: "Pipeline failed. Please check the logs.")
    }
}
}
