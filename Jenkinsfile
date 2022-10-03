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
        stage("Tag docker image") {
            steps {
                sh "docker tag cardb:0.0.1-SNAPSHOT $DOCKER_HUB_LOGIN_USER/cardb"
            }
        }
        stage("Push Docker image to Docker Hub") {
            steps {
                sh "docker login --username $DOCKER_HUB_LOGIN_USER --password $DOCKER_HUB_LOGIN_PASS"
                sh "docker push $DOCKER_HUB_LOGIN_USER/cardb"
            }
        }
        stage("Tag docker image to AWS") {
            steps {
                sh "docker tag cardb:0.0.1-SNAPSHOT $AWS_ECR_HOST/cardb"
            }
        }
        stage("Push Docker image to ECR") {
            steps {
                sh "docker login --username=AWS --password=$AWS_ECR_PASS $AWS_ECR_HOST"
                sh "docker push $AWS_ECR_HOST/cardb"
            }
        }
    }
}

