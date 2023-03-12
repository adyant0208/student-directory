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
        // stage("SonarQube Analysis") {
        //     environment {
        //         scannerHome = tool "SonarScannerServer"
        //     }
        //     steps {
        //         scripts {
        //             withSonarQubeEnv(installationName: 'SonarScannerServer', credentialsId: 'sonar-api') {
        //             sh './gradlew sonarqube'
        //             }
        //         }
        //     }
        // }
        //-D sonar.sources=student-directory/src/main/ \
        // -D sonar.tests=student-directory/src/test \


        stage('CODE ANALYSIS with SONARQUBE') {
          
		  environment {
             scannerHome = tool 'SonarScannerServer'
          }

          steps {
            script {
                withSonarQubeEnv(credentialsId: 'sonarserver') {
                sh 'chmod +x gradlew'
                sh './gradlew sonarqube'
            }

            timeout(time: 10, unit: 'MINUTES') {
               waitForQualityGate abortPipeline: true
            }
            
            }
          }
        }
    }
}