pipeline {
	agent any
	stages {
    	stage('Clean Workspace') {
        	steps {
            	deleteDir()
        	}
    	}
        
    	stage('GPT Review') {
        	steps {
            	script {
                	checkout([
                    	$class: 'GitSCM',
                    	branches: [[name: '*/main']], // Specify branch if needed
                    	userRemoteConfigs: [[
                        	url: 'https://github.com/thecoder-10/Copilot-Review-React' //Replace with your own repository URL
                    	]]
                	])

                	withCredentials([string(credentialsId: 'OPENAI_API_KEY', variable: 'OPENAI_API_KEY')]){
                    	withCredentials([string(credentialsId: 'GH_TOKEN', variable: 'GH_TOKEN')]) {

                        	// GPTSCript reviews the code
                        	REVIEW = sh(script: "gptscript codereview.gpt --PR_URL=${PR_URL}", returnStdout: true).trim()                     	 

                        	// Construct the JSON payload using Groovy's JSON library
                        	def jsonPayload = groovy.json.JsonOutput.toJson([body: REVIEW])

                        	// Post the review comment to the GitHub PR
                        	sh """
    curl -H "Authorization: token ${GH_TOKEN}" \
         -H "Content-Type: application/json" \
         -X POST -d '${jsonPayload}' \
         '${PR_COMMENTS_URL}'
"""


                	}
                	}
            	}
        	}
    	}

    	stage('Check PR Status') {
        	steps {
            	script {

                	// Check if REVIEW contains 'Require Changes'
                	if (REVIEW.contains('Require Changes')) {
                    	echo 'Code Requires Changes'
                    	currentBuild.result = 'FAILURE' // Mark the build as failed
                    	error 'Code Requires Changes' // Terminate the build with an error
                	}

               	 
                	// Check if REVIEW contains 'Approved'
                	if (REVIEW.contains('Approved')) {
                    	echo 'Code Approved'
                	}
            	}
        	}
    	}
	}
}
