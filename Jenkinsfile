pipeline {
    agent any

    triggers {
        githubPullRequest {
            orgWhitelist(['thecoder-10'])
            allowMembersOfWhitelistedOrgsAsAdmin()
        }
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
                    def lang = "js"  // Change this based on your project (e.g., "py" for Python)

                    if (lang == "js") {
                        sh 'npm install eslint && npx eslint .'
                    } else if (lang == "py") {
                        sh 'pip install pylint && pylint **/*.py'
                    } else {
                        echo "No static analysis configured for this language"
                    }
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
            error("Check the logs for details!")
        }
    }
}
