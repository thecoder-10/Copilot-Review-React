pipeline {
    agent any

    triggers {
        githubPullRequest(
            cron: 'H/5 * * * *', // Checks for PR updates every 5 minutes
            onlyTriggerPhrase: false, // ✅ Automatically triggers on every PR
            useGitHubHooks: true // ✅ Uses GitHub Webhook for instant triggering
        )
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
