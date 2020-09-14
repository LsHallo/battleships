pipeline {
   environment {
      registry = 'hub.lshallo.eu/battleships'
   }
   agent any
   stages {
       stage('Cloning git') {
           steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CleanBeforeCheckout', deleteUntrackedNestedRepositories: true]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/LsHallo/battleships.git']]])
          }
       }
       stage('Build image and push to docker hub') {
          steps{
            script {
                withCredentials([usernamePassword(credentialsId: 'lshalloDockerHub', passwordVariable: 'password', usernameVariable: 'username')]) {
                    sh('docker login --username $username --password $password hub.lshallo.eu')
                }

                sh('docker buildx rm battleshipsbuilder || true')
                sh('docker buildx create --name battleshipsbuilder --use')
                sh("docker buildx build --tag $registry:latest --platform=linux/amd64 --push .")
                sh('docker buildx rm battleshipsbuilder || true')
            }
          }
        }
   }
}
