pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/poojkulk/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("poojakk123/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
//     stage('Deploy App') {
//       steps {
//         script {
//           kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
//         }
//       }
//     }
    stage('Deploy App') {
      steps {
        script {
         withKubeConfig([credentialsId: 'kubeconfig', serverUrl: 'https://127.0.0.1:32779']) {
            bat 'kubectl apply -f hellowhale.yml'
    }
        }
      }
    }

  }

}
