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
                script {
                    def mavenPom = readMavenPom file: 'pom.xml' // Used to get data from pom.xml
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'my-app', 
                            classifier: '', 
                            file: "target/my-app-${mavenPom.version}.jar", 
                            type: 'jar'
                        ]
                    ], 
                    credentialsId: '33b4f031-8bab-4f9f-976d-cf771b6035cb', 
                    groupId: 'com.mycompany.app', 
                    nexusUrl: 'localhost:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'my-app-release', 
                    version: "${mavenPom.version}"
                }
            }
        }
        stage('Generate report') {
            steps {
                script {
                        
                    echo "${currentBuild.result} == 'SUCCESS'"
                    if("${currentBuild.result}" == "SUCCESS") {
                        sh "curl -v -H 'Content-Type: application/json' -X POST -d '{Build Status: ${currentBuild.result}' http://localhost:1080"
                        //sh 'curl -v -H "Content-Type: application/json" -X POST -d '{"Build Status":"${currentBuild.result}"}' http://localhost:1080'
                    }
                }
            }
        }
    }
    post('Generate report') { 
	always {
        steps {
            echo "${currentBuild.result}"
            if("${currentBuild.result}" == "SUCCESS") {
                echo "if"
            }
            else {
                echo 'else'
            }
        }
			script{
				emailext subject: "Automation Result: Job '${env.JOB_NAME} - ${env.BUILD_NUMBER}'", 
				body: "${env.BUILD_URL} has result ${currentBuild.result}",
				to:'$DEFAULT_RECIPIENTS'
			}
		}
    }
}
