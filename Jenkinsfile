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
                echo 'sh "docker tag cardb:0.0.1-SNAPSHOT:$env.BUILD_TAG $DOCKER_HUB_LOGIN_USER/cardb:$env.BUILD_TAG"'
            }
        }
        stage("Push Docker image to Docker Hub") {
            steps {
                echo 'sh "docker login --username $DOCKER_HUB_LOGIN_USER --password $DOCKER_HUB_LOGIN_PASS"'
                echo 'sh "docker push $DOCKER_HUB_LOGIN_USER/cardb:$env.BUILD_TAG"'
            }
        }
        stage("Tag docker image to AWS") {
            steps {
                echo 'sh "docker tag cardb:0.0.1-SNAPSHOT:$env.BUILD_TAG $AWS_ECR_HOST/cardb:$env.BUILD_TAG"'
            }
        }
        stage("Push Docker image to ECR") {
            steps {
                echo 'sh "docker login --username=AWS --password=$AWS_ECR_PASS $AWS_ECR_HOST"'
                echo 'sh "docker push $AWS_ECR_HOST/cardb:$env.BUILD_TAG"'
            }
        }
    }
    post {
        always {
            echo "curl github"
            echo "archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true"
            echo "Build completed with status: ${currentBuild.result}"
        }
        success {
            echo "sh 'curl -XPOST"
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

