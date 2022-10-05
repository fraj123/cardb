pipeline {
    agent {
        docker { image 'openjdk:17.0.2' }
    }
    environment {
            BUILD_TAG = "build_${BUILD_NUMBER}"
        }
    stages {
        
        stage('Show Work Branch') {
            steps {
                echo 'Building...' + env.BRANCH_NAME
            }
        }
        stage('Code Coverage Test') {
            steps {
                echo "sonar"
            }
        }
        stage("Unit Test stage") {
            steps {
                echo "./mvnw test -Dsnyk.skip"
            }
        }
        stage("Security Test Stage") {
            steps {
                echo "security"
            }
        }
        stage("Build") {
            steps {
                echo "./mvnw install -Dsnyk.skip"
            }
        }
        stage("Build Docker Image") {
            steps {
                echo "./mvnw spring-boot:build-image -Dsnyk.skip"
            }
        }
        stage("Tag docker image") {
            steps {
                echo "/cardb:$env.BUILD_TAG"
            }
        }
        stage("Push Docker image to Docker Hub") {
            steps {
                echo "Login"
                echo "docker push /cardb:$env.BUILD_TAG"
            }
        }
        stage("Tag docker image to AWS") {
            steps {
                echo "docker tag cardb:0.0.1-SNAPSHOT:$env.BUILD_TAG /cardb:$env.BUILD_TAG"
            }
        }
        stage("Push Docker image to ECR") {
            steps {
                echo "docker login"
                echo "docker push /cardb:$env.BUILD_TAG"
            }
        }
    }
    post {
        always {
            echo "curl github"
            echo "artifact"
            echo "Build completed with status: ${currentBuild.result}"
        }
        success {
            echo "XPOST"
            office365ConnectorSend color: '#86BC25', status: currentBuild.result, webhookUrl: "${URL_WEBHOOK_C}",
            message: "Test Successful: ${JOB_NAME} - ${currentBuild.displayName}<br>Pipeline duration: ${currentBuild.durationString.replace(' and counting', '')}"
        }
        unstable {
           office365ConnectorSend color: '#FFE933', status: currentBuild.result, webhookUrl: "${URL_WEBHOOK_C}",
           message: "Successfully Build but Unstable. Unstable means test failure, code violation, push to remote failed etc. : ${JOB_NAME} - ${currentBuild.displayName}<br>Pipeline duration: ${currentBuild.durationString.replace(' and counting', '')}"
        }
        failure {
            office365ConnectorSend color: '#ff0000', status: currentBuild.result, webhookUrl: "${URL_WEBHOOK_C}",
            message: "Build Failed: ${JOB_NAME} - ${currentBuild.displayName}<br>Pipeline duration: ${currentBuild.durationString.replace(' and counting', '')}"
        }
    }
}

