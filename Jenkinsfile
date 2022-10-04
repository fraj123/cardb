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
        stage("Push Docker image to ECR") {
            steps {
                echo 'sh "docker login --username=AWS --password=$AWS_ECR_PASS $AWS_ECR_HOST"'
                echo 'sh "docker push $AWS_ECR_HOST/cardb"'
            }
        }
    }
    post {
        success {
            office365ConnectorSend color: '#86BC25', status: currentBuild.result, webhookUrl: "${wbhk_martin}",
            message: "Test Successful: ${JOB_NAME} - ${currentBuild.displayName}<br>Pipeline duration: ${currentBuild.durationString.replace(' and counting', '')}"
        }
        unstable {
           office365ConnectorSend color: '#FFE933', status: currentBuild.result, webhookUrl: "${wbhk_martin}",
           message: "Successfully Build but Unstable. Unstable means test failure, code violation, push to remote failed etc. : ${JOB_NAME} - ${currentBuild.displayName}<br>Pipeline duration: ${currentBuild.durationString.replace(' and counting', '')}"
        }
        failure {
            office365ConnectorSend color: '#ff0000', status: currentBuild.result, webhookUrl: "${wbhk_martin}",
            message: "Build Failed: ${JOB_NAME} - ${currentBuild.displayName}<br>Pipeline duration: ${currentBuild.durationString.replace(' and counting', '')}"
        }
        always {
            echo "Build completed with status: ${currentBuild.result}"
        }
    }
}

