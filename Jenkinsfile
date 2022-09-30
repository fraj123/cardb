pipeline {
    agent {
        docker { image 'openjdk:17.0.2' }
    }
    stages {
        stage('Build with unit testing') {
            steps {
                echo 'Building...' + env.BRANCH_NAME
            }
        }
        stage('Security Stage') {
            steps {
                echo "Testing security with snyk"
            }
        }
        stage('Code coverage Test') {
            steps {
                echo "Code coverage test"
            }
        }
        stage("Test stage") {
            steps {
                sh "./mvnw test"
            }
        }
        stage("Build") {
            steps {
                sh "./mvnw compile"
            }
        }
    }
}

