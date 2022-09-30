pipeline {
    agent {
        docker { image 'maven:3.8.6' }
    }
    stages {
        stage('Build with unit testing') {
            steps {
                echo 'Building...' + env.BRANCH_NAME
            }
        }
        stage("Test stage") {
            steps {
                sh "./mvnw test"
            }
        }
    }
}

