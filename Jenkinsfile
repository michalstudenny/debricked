pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub2')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t michalstudenny/jenkins-docker-hub .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
      
              stage('Vulnerability scan') {

            environment {

                DEBRICKED_TOKEN = credentials('DEBRICKED_TOKEN')

            }



            agent {

                docker {

                    image 'debricked/debricked-cli'

                    args '--entrypoint="" -v ${WORKSPACE}:/data -w /data'

                }

            }

            steps {

                sh 'bash /home/entrypoint.sh debricked:scan "" "$DEBRICKED_TOKEN" example-jenkins "$GIT_COMMIT" null cli'

            }

        }
    stage('Push') {
      steps {
        sh 'docker push michalstudenny/jenkins-docker-hub'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
