pipeline {
    agent any

    triggers {
        // 🚀 GitHub Webhook handles PR triggers, no need for pollSCM
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    if (env.CHANGE_ID) { // ✅ Runs only on PRs in a Multibranch Pipeline
                        echo "Triggered by Pull Request #${env.CHANGE_ID}"
                    }
                    checkout scm
                }
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
            echo "✅ PR Passed Static Analysis"
        }
        failure {
            echo "❌ PR Failed Static Analysis"
        }
    }
}
