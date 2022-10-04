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
                office365ConnectorSend webhookUrl: "https://jalauniv.webhook.office.com/webhookb2/a1ce7da2-8273-4129-a365-d107fdf19abd@e342d848-a6cb-46aa-ac19-4800f62fb836/JenkinsCI/9eb183eeee2f4664849a1dd0ace1dcf3/30d2a5db-3610-46ca-868b-e33b81596709",
                                        message: 'Build image, luis',
                                        status: 'Success'
            }
        }
        stage("Tag docker image") {
            steps {
                echo 'sh "docker tag cardb:0.0.1-SNAPSHOT $DOCKER_HUB_LOGIN_USER/cardb"'
                office365ConnectorSend webhookUrl: "https://jalauniv.webhook.office.com/webhookb2/a1ce7da2-8273-4129-a365-d107fdf19abd@e342d848-a6cb-46aa-ac19-4800f62fb836/JenkinsCI/9eb183eeee2f4664849a1dd0ace1dcf3/30d2a5db-3610-46ca-868b-e33b81596709",
                                        message: 'Tag docker image, luis',
                                        status: 'Success'
            }
        }
       
    }
}

