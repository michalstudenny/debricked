pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
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
        
        stage('Deploy') {
            steps {
                sh 'mvn deploy'
            }
        }
    }
}
