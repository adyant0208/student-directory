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
        stage('CODE ANALYSIS with SONARQUBE') {
          
		  environment {
             scannerHome = tool 'SonarScannerServer'
          }

          steps {
            script {
                withSonarQubeEnv(credentialsId: 'sonar-auth-token') {
                sh 'chmod +x gradlew'
                sh './gradlew sonarqube'
                }
            }
          }
        }
        stage("Pushing artifact to Nexus") {
            steps {
                script {
                    nexusArtifactUploader artifacts: 
                    [[artifactId: 'student-directory', 
                    classifier: '', 
                    file: 'target/student-directory/build/libs', 
                    type: 'jar']], 
                    credentialsId: 'nexus-auth', 
                    groupId: 'student-directory', 
                    nexusUrl: 'ec2-43-207-173-150.ap-northeast-1.compute.amazonaws.com:8081/', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'student-directory-release', 
                    version: '1.1.0'
                }
            }
        }
    }
}