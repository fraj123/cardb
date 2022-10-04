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
                office365ConnectorSend webhookUrl: "https://jalauniv.webhook.office.com/webhookb2/d7ad5541-2891-480c-aa17-f8fb042005a2@e342d848-a6cb-46aa-ac19-4800f62fb836/IncomingWebhook/1d04db455ead46c0af7f4316aa751afb/e39755ff-393d-4e19-8443-19f3bca17ff9",
                                        message: 'Build image, luis',
                                        status: 'Success'
            }
        }
        stage("Tag docker image") {
            steps {
                echo 'sh "docker tag cardb:0.0.1-SNAPSHOT $DOCKER_HUB_LOGIN_USER/cardb"'
                office365ConnectorSend webhookUrl: "https://jalauniv.webhook.office.com/webhookb2/d7ad5541-2891-480c-aa17-f8fb042005a2@e342d848-a6cb-46aa-ac19-4800f62fb836/IncomingWebhook/1d04db455ead46c0af7f4316aa751afb/e39755ff-393d-4e19-8443-19f3bca17ff9",
                
                                        message: 'Tag docker image, luis',
                                        status: 'Success'
            }
        }
       
    }
}

