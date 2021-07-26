pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    triggers {
        githubPush()
    } 
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Upload JAR to Nexus') {
            steps {
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'my-app', 
                        classifier: '', 
                        file: 'target/my-app-1.1.0.jar', 
                        type: 'jar'
                    ]
                ], 
                credentialsId: '33b4f031-8bab-4f9f-976d-cf771b6035cb', 
                groupId: 'com.mycompany.app', 
                nexusUrl: 'localhost:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'my-app-release', 
                version: '1.1.0'
            }
        }
    }
    post('Generate report') { 
		always {
			script{
                cucumber fileIncludePattern: '**/cucumber-default-reports/*.json', sortingMethod: 'ALPHABETICAL'
                
                junit '**/cucumber-default-reports/*.xml' / / Must have this step, otherwise there is no data                  
                emailext subject: "Automation Result5: Job '${env.JOB_NAME} - ${env.BUILD_NUMBER}'",
                   body:'''${SCRIPT,template="groovy-html-larry-refactor.template"}''',
                    to:'$DEFAULT_RECIPIENTS'
                
				emailext subject: "Automation Result6: Job '${env.JOB_NAME} - ${env.BUILD_NUMBER}'",
                   body:''' ${SCRIPT,template="groovy-html-refactor.template"}''',
                    to:'$DEFAULT_RECIPIENTS'

                emailext subject: "Automation Result: Job '${env.JOB_NAME} - ${env.BUILD_NUMBER}'", 
                body:'''  
                      total:${TEST_COUNTS,var="total"},
                      pass:${TEST_COUNTS,var="pass"},
                      fail:${TEST_COUNTS,var="fail"}
                   ''',
                to:'$DEFAULT_RECIPIENTS'
            }
        }
	}
}