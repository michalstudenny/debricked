pipeline {
    agent any
    
    environment {
        // Define environment variables here
        MAVEN_HOME = tool 'Maven-3.6.3'
        MAVEN_OPTS = '-Dmaven.repo.local=${HOME}/.m2/repository'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }
        
        stage('Test') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn test"
            }
        }
        
        stage('Deploy') {
            steps {
                withMaven(maven: 'Maven-3.6.3', mavenSettingsConfig: 'maven-settings') {
                    sh "${MAVEN_HOME}/bin/mvn deploy"
                }
            }
        }
    }
    
    post {
        always {
            junit 'target/surefire-reports/**/*.xml'
        }
    }
}
