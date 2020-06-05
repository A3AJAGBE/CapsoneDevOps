pipeline {

     agent any

      stage('Start Pipeline') {
        steps {
           echo "This is the first step of the build"
           echo "Start Build"
          }
    }

      stages {
          stage('Lint HTML') {
              steps {
                  sh 'tidy -q -e *.html'
                  sh 'hadolint Dockerfile'
              }
         }
    }

    stage('Build image') {
      steps {
          script {
              dockerImage = docker.build('a3ajagbe/kubernetes-clusters-demo:lastest')
              docker.withRegistry('', 'docker') {
                  dockerImage.push()
              }
          }
      }
    }

    stage('Deploy App') {
      steps {
                withAWS(credentials: 'aws-cred', region: us-west-2) {
                    sh 'aws eks --region=us-west-2 update-kubeconfig --name KubsCluster'
                    sh 'kubectl apply -f kubernetes-config.yml'
                }
            }
      }

}
