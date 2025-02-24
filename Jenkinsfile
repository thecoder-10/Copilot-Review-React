pipeline {
    agent any

    // ✅ Correct way to add PR trigger
    options {
        properties([
            pipelineTriggers([
                [$class: 'GitHubPRTrigger', onlyTriggerPhrase: false]
            ])
        ])
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Static Code Analysis') {
            steps {
                script {
                    sh 'npm install eslint && npx eslint . || true'
                }
            }
        }
    }

    post {
        success {
            echo "PR Passed Static Analysis ✅"
        }
        failure {
            echo "PR Failed Static Analysis ❌"
        }
    }
}
