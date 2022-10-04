pipeline {
    agent {
        docker { image 'openjdk:17.0.2' }
    }
    options {
        office365ConnectorWebhooks([
            [name: "Office 365", url: "${URL_WEBHOOK_C}", notifyBackToNormal: true, notifyFailure: true, notifyRepeatedFailure: true, notifySuccess: true, notifyAborted: true]
        ])
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
                echo 'sh "docker tag cardb:0.0.1-SNAPSHOT $DOCKER_HUB_LOGIN_USER/cardb"'
            }
        }
        stage("Push Docker image to Docker Hub") {
            steps {
                echo 'sh "docker login --username $DOCKER_HUB_LOGIN_USER --password $DOCKER_HUB_LOGIN_PASS"'
                echo 'sh "docker push $DOCKER_HUB_LOGIN_USER/cardb"'
            }
        }
        stage("Tag docker image to AWS") {
            steps {
                echo 'sh "docker tag cardb:0.0.1-SNAPSHOT $AWS_ECR_HOST/cardb"'
            }
        }
        stage("Send notification on Teams") {
            steps {
                echo 'Send notification 2'
                office365ConnectorSend webhookUrl: "${URL_WEBHOOK_C}",
                message: 'Code is deployed',
                status: 'Success'            
            }
        }
        stage("Push Docker image to ECR") {
            steps {
                echo 'sh "docker login --username=AWS --password=$AWS_ECR_PASS $AWS_ECR_HOST"'
                echo 'sh "docker push $AWS_ECR_HOST/cardb"'
            }
        }
    }
}

