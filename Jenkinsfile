pipeline {
    agent {
        docker { image 'openjdk:17.0.2' }
    }
    stages {
        stage('Build with unit testing') {
            steps {
                echo 'Building...' + env.BRANCH_NAME
            }
        }
        stage('Security Stage') {
            steps {
                echo "Testing security with snyk"
            }
        }
        stage('Code coverage Test') {
            steps {
                echo "Todo install sonarqube server"
            }
        }
        stage("Test stage") {
            steps {
                sh "./mvnw test"
            }
        }
        stage("Build") {
            steps {
                sh "./mvnw compile"
            }
        }
    }
    post {
        success {
            office365ConnectorSend color: '#86BC25', status: currentBuild.result, webhookUrl: "${ WEBHOOK_URL_JOSE }",
            message: "Test Successful: ${JOB_NAME} - ${currentBuild.displayName}<br>Pipeline duration: ${currentBuild.durationString.replace(' and counting', '')}"
        }
        unstable {
            office365ConnectorSend color: '#FFE933', status: currentBuild.result, webhookUrl: "${ WEBHOOK_URL_JOSE }",
            message: "Successfully Build but Unstable. Unstable means test failure, code violation, push to remote failed etc. : ${JOB_NAME} - ${currentBuild.displayName}<br>Pipeline duration: ${currentBuild.durationString.replace(' and counting', '')}"
        }
        failure {
            office365ConnectorSend color: '#ff0000', status: currentBuild.result, webhookUrl: "${ WEBHOOK_URL_JOSE }",
            message: "Build Failed: ${JOB_NAME} - ${currentBuild.displayName}<br>Pipeline duration: ${currentBuild.durationString.replace(' and counting', '')}"
        }
        always {
            echo "Build completed with status: ${currentBuild.result}"
        }
    }
}

