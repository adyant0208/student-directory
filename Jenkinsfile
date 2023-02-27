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


        stage('CODE ANALYSIS with SONARQUBE') {
          
		  environment {
             scannerHome = tool 'SonarScannerServer'
          }

          steps {
            script {
                withSonarQubeEnv(credentialsId: 'SonarServer') {
               sh '''/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarScannerServer/bin/sonar-scanner \
     -D sonar.projectVersion=0.0.1-SNAPSHOT \
       -D sonar.login=admin \
      -D sonar.password=Welcome4$ \
      -D sonar.projectBaseDir=/var/lib/jenkins/workspace/student-directory/ \
        -D sonar.projectKey=student-directory \
        -D sonar.sourceEncoding=UTF-8 \
        -D sonar.language=java \
        -D sonar.sources=student-directory/src/main/ \
        -D sonar.tests=student-directory/src/test \
        -D sonar.host.url=http://ec2-54-249-68-48.ap-northeast-1.compute.amazonaws.com:9000/'''
            }

            timeout(time: 10, unit: 'MINUTES') {
               waitForQualityGate abortPipeline: true
            }
            
            }
          }
        }
    }
}