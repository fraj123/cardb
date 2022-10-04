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
            office365ConnectorSend color: '#83eb34', status: currentBuild.result, webhookUrl: "${wbhk_martin}",
            message: "Test Successful: ${JOB_NAME} - ${currentBuild.displayName}<br>Pipeline duration: ${currentBuild.durationString.replace(' and counting', '')}"
        }
        unstable {
            office365ConnectorSend color: '#d68f31', status: currentBuild.result, webhookUrl: "${wbhk_martin}",
            message: "Successfully Build but Unstable. Unstable means test failure, code violation, push to remote failed etc. : ${JOB_NAME} - ${currentBuild.displayName}<br>Pipeline duration: ${currentBuild.durationString.replace(' and counting', '')}"
        }
        failure {
            office365ConnectorSend color: '#e63415', status: currentBuild.result, webhookUrl: "${wbhk_martin}",
            message: "Build Failed: ${JOB_NAME} - ${currentBuild.displayName}<br>Pipeline duration: ${currentBuild.durationString.replace(' and counting', '')}"
        }
        always {
            echo "Build completed with status: ${currentBuild.result}"
        }
    }
}

