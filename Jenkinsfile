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
        stage('Code Coverage Test') {
            steps {
                sh "./mvnw clean install -DskipTests=true -Dmaven.test.failure.ignore=true sonar:sonar -Dsonar.projectKey=cardb -Dsonar.host.url=$env.SONAR_HOST -Dsonar.login=$env.SONAR_TOKEN"
            }
        }
        stage("Unit Test stage") {
            steps {
                sh "./mvnw test -Dsnyk.skip"
            }
        }
        stage("Security Test Stage") {
            steps {
                sh "./mvnw snyk:test"
            }
        }
        stage("Build") {
            steps {
                sh "./mvnw compile"
            }
        }
    }
}

