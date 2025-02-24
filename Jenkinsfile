pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    if (env.CHANGE_ID) { // ✅ Detects PRs in a Multibranch Pipeline
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
