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
                        file: 'target/my-app-1.2.0.jar', 
                        type: 'jar'
                    ]
                ], 
                credentialsId: '33b4f031-8bab-4f9f-976d-cf771b6035cb', 
                groupId: 'com.mycompany.app', 
                nexusUrl: 'localhost:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'my-app-release', 
                version: '1.2.0'
            }
        }
    }
    post('Generate report') { 
		always {
			script{
                emailext subject: "Automation Result: Job '${env.JOB_NAME} - ${env.BUILD_NUMBER}'", 
                body: "${env.BUILD_URL} has result ${currentBuild.result}",
                to:'$DEFAULT_RECIPIENTS'
            }
        }
	}
}