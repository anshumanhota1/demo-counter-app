pipeline {
    agent any 

    tools {
        maven 'my-maven'  // This references the Maven installation configured in Jenkins
    }

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/anshumanhota1/demo-counter-app.git'
            }
        }

        stage('Unit Testing') {
            steps {
                sh "${tool 'my-maven'}/bin/mvn test"
            }
        }

        stage('Integration Testing') {
            steps {
                sh "${tool 'my-maven'}/bin/mvn verify -DskipUnitTests"
            }
        }

        stage('Maven Build') {
            steps {
                sh "${tool 'my-maven'}/bin/mvn clean install"
            }
        }

        stage('Static Code Analysis') {
            steps {
                withSonarQubeEnv(credentialsId: 'sonar-api') {
                    sh "${tool 'my-maven'}/bin/mvn clean package sonar:sonar"
                }
            }
        }

        stage('Quality Gate Status') {
            steps {
                waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
            }
        }
    }
}
