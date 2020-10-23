pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        checkout scm
        //git url:'https://github.com/kunal-parsewar/hellowhale.git', branch:'master'
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
                    docker.withRegistry('', 'DockerHub') {
                    myapp.push("${env.BUILD_NUMBER}")
                    myapp.push("latest")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          // below two commands are not working
          //kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "kubeconfig")
          //kubernetesDeploy([kubeconfigId: 'kubeconfig' , configs: 'hellowhale.yml'])
          
          //kubernetesDeploy(credentialsType: 'KubeConfig', kubeConfig: [path: 'kubeconfig.yaml'], configs: '**/hellowhale.yml', enableConfigSubstitution: false )
          
          withKubeConfig ([credentialsId: 'kubeconfigyaml' , serverUrl: 'https://172.31.8.223:6443'])
          {
            sh 'kubectl create -f $WORKSPACE/hellowhale.yml'
          }
          
        }
      }
    }

  }

}
