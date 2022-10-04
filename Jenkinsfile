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

    options {
        office365ConnectorWebhooks([
            [name: "Office 365", url: "${https://jalauniv.webhook.office.com/webhookb2/a1ce7da2-8273-4129-a365-d107fdf19abd@e342d848-a6cb-46aa-ac19-4800f62fb836/JenkinsCI/1fff2e526efb4d84b7a870c3430db177/511d3a02-cd7c-42b4-a2e8-c98cb5735dca}", notifyBackToNormal: true, notifyFailure: true, notifyRepeatedFailure: true, notifySuccess: true, notifyAborted: true]
        ])
    }

    stages {     
        stage("Deploy")
        {
            steps {
                office365ConnectorSend webhookUrl: "${https://jalauniv.webhook.office.com/webhookb2/a1ce7da2-8273-4129-a365-d107fdf19abd@e342d848-a6cb-46aa-ac19-4800f62fb836/JenkinsCI/1fff2e526efb4d84b7a870c3430db177/511d3a02-cd7c-42b4-a2e8-c98cb5735dca}",
                message: 'Code is deployed',
                status: 'Success'            
            }
        }
    }
}
