pipeline {
    agent any
  environment {
    DOCKER_IMAGE_NAME = "tunuofmrec/myweb"
      }
  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/tunuofmrec/playjenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubeconfig")
        }
      }
    }

  }
  }
