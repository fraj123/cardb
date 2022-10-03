pipeline {
    agent {
        docker { image 'openjdk:17.0.2' }
    }
    stages {
        stage('Show Work Branch') {
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
                sh "./mvnw install -Dsnyk.skip"
            }
        }
        stage("Build Docker Image") {
            steps {
                sh "./mvnw spring-boot:build-image -Dsnyk.skip"
            }
        }
        stage("Install docker") {
            steps {
                sh "apt update"
                sh "apt install docker-ce"
            }
        }
        stage("Tag docker image") {
            steps {
                sh "docker tag cardb:0.0.1-SNAPSHOT api/cardb"
            }
        }
        stage("Push Docker image to Docker Hub") {
            steps {
                sh "docker login --username=$DOCKER_HUB_LOGIN_USER --password=$DOCKER_HUB_LOGIN_PASS"
                sh "docker push api/cardb"
            }
        }
        stage("Push Docker image to ECR") {
            steps {
                sh "docker login --username=AWS --password=$AWS_ECR_PASS $AWS_ECR_HOST"
                sh "docker push api/cardb"
            }
        }
    }
}

