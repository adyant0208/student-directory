pipeline {
    agent any 
    stages {
        stage("Git Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/adyant0208/student-directory.git'
            }
        }
         stage("Gradle Build") {
            steps {
                sh './gradlew clean build'
            }
        }
        stage("Gradle Test") {
            steps {
                sh './gradlew test'
            }
        }
        stage("Gradle Check") {
            steps {
                sh './gradlew check'
            }
        }
        stage("Gradle BuildEnvironment") {
            steps {
                sh './gradlew buildEnvironment'
            }
        }
        stage("SonarQube Analysis") {
            steps {
                scripts {
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        sh './gradlew sonarqube'
                    }
                }
            }
        }
    }
}