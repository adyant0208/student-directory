pipeline {
    agent any
    tools {
        gradle '7.6.1' 
    } 
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
                    withSonarQubeEnv(installationName: 'SonarQubeScanner', credentialsId: 'jenkins') {
                    sh './gradlew sonarqube \
                  -Dsonar.projectKey=${serviceName} \
                  -Dsonar.host.url=${env.SONAR_HOST_URL} \
                  -Dsonar.login=${env.SONAR_AUTH_TOKEN} \
                  -Dsonar.projectName=${serviceName} \
                  -Dsonar.projectVersion=${BUILD_NUMBER}'
                    }
                }
            }
        }
    }
}