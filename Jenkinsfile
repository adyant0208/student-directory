pipeline {
    agent any 
    stages {
        stage("Git Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/adyant0208/student-directory.git'
            }
        }
         stage("Unit Testing") {
            steps {
                sh 'build'
            }
        }
    }
}