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
                sh "./mvnw clean install -Dsnyk.skip -DskipTests=true -Dmaven.test.failure.ignore=true sonar:sonar -Dsonar.projectKey=cardb -Dsonar.host.url=$env.SONAR_HOST -Dsonar.login=$env.SONAR_TOKEN"
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
        stage("Tag docker image") {
            steps {
                sh "docker tag cardb:0.0.1-SNAPSHOT $DOCKER_HUB_LOGIN_USER/cardb"
            }
        }
        stage("Push Docker image to Docker Hub") {
            steps {
                sh "docker login --username $DOCKER_HUB_LOGIN_USER --password $DOCKER_HUB_LOGIN_PASS"
                sh "docker push $DOCKER_HUB_LOGIN_USER/cardb"
                office365ConnectorSend webhookUrl: "$TEAM_WEBHOOK",
                                        message: 'Image has been uploaded to DOcker Hub',
                                        status: 'Success'
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
                office365ConnectorSend webhookUrl: "$TEAM_WEBHOOK",
                                        message: 'Image has been uploaded to ECR',
                                        status: 'Success'
            }
        }
    }

    post {
        always {
            curl github
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        }
        success {
            sh 'curl -XPOST -H "Authorization:token $GITHUB_TOKEN" --data "**/target/*.jar"' https://github.com/fraj123/cardb/releases
            office365ConnectorSend webhookUrl: "$TEAM_WEBHOOK",
                                    message: 'The process has been finished without any error',
                                    status: 'Success'
        }
        failure {
            office365ConnectorSend webhookUrl: "$TEAM_WEBHOOK",
                                    message: 'Error',
                                    status: 'Failure'
        }
    }
}

