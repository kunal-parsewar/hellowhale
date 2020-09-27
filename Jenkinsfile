pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/kunal-parsewar/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("jdevsucks/bluewhale")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('', 'jdevsucks') {
                    myapp.push("${env.BUILD_NUMBER}")
                    myapp.push("latest")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "kubeconfigdemo")
        }
      }
    }

  }

}
