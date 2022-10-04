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
                office365ConnectorSend webhookUrl: "${URL_WEBHOOK}",
                                        message: 'Build image, luis',
                                        status: 'Success'
            }
        }
        stage("Tag docker image") {
            steps {
                echo 'sh "docker tag cardb:0.0.1-SNAPSHOT $DOCKER_HUB_LOGIN_USER/cardb"'
                office365ConnectorSend webhookUrl: "${URL_WEBHOOK}",
                
                                        message: 'Tag docker image, luis',
                                        status: 'Success'
            }
        }
       
    }
}

