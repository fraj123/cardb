pipeline {
    agent {
        docker { image 'openjdk:17.0.2' }
    }
    options {
        office365ConnectorWebhooks([
            [name: "Office 365", url: "https://jalauniv.webhook.office.com/webhookb2/d7ad5541-2891-480c-aa17-f8fb042005a2@e342d848-a6cb-46aa-ac19-4800f62fb836/IncomingWebhook/1d04db455ead46c0af7f4316aa751afb/e39755ff-393d-4e19-8443-19f3bca17ff9", notifyBackToNormal: true, notifyFailure: true, notifyRepeatedFailure: true, notifySuccess: true, notifyAborted: true]
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
                office365ConnectorSend webhookUrl: "https://jalauniv.webhook.office.com/webhookb2/d7ad5541-2891-480c-aa17-f8fb042005a2@e342d848-a6cb-46aa-ac19-4800f62fb836/IncomingWebhook/1d04db455ead46c0af7f4316aa751afb/e39755ff-393d-4e19-8443-19f3bca17ff9",
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

