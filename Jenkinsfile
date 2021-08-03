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
                    //def mavenPom = readMavenPom file: 'pom.xml' // Used to get data from pom.xml
                    //def props = readProperties file: 'src/main/resources/application.properties'
                    def props = readProperties file: '~/.bash_profile'
                    env.VERSION = props.VERSION
                    echo "version is ${VERSION}"
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'my-app', 
                            classifier: '', 
                            file: "target/my-app-${VERSION}.jar", 
                            type: 'jar'
                        ]
                    ], 
                    credentialsId: '33b4f031-8bab-4f9f-976d-cf771b6035cb', 
                    groupId: 'deviceMake.deviceModel.qa', 
                    nexusUrl: 'localhost:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'my-app-release', 
                    version: "${VERSION}"
                }
            }
        }
    }
    post('Generate report') { 
	    always {
            script {
                // Sends HTTP POST request in JSON format
                //def mavenPom = readMavenPom file: 'pom.xml'
                def props = readProperties file: '~/.bash_profile'
                env.VERSION = props.VERSION
                echo "version is ${VERSION}"
                def jobdetails = "${env.JOB_NAME}-${env.BUILD_NUMBER}"
                def jobstatus = "${currentBuild.result}"
                def myJson = "{\"Job details\": \"${jobdetails}\", \"Build status\": \"${jobstatus}\", \"Version\": \"${VERSION}\"}";
                echo "${myJson}"
                if("${currentBuild.result}" == "SUCCESS") {
                    def destination_ip = "http://localhost:1080" // destination_ip of the server which listens the requests
                    sh "curl -v -H 'Content-Type: application/json' -X POST -d '${myJson}' ${destination_ip}" 
                }
                else {
                    echo 'Build failed/aborted'
                }

                // Sends an email notification to the developer
				emailext subject: "Automation Result: Job '${env.JOB_NAME} - ${env.BUILD_NUMBER}'", 
				body: "${env.BUILD_URL} has result ${currentBuild.result}",
				to:'$DEFAULT_RECIPIENTS'
			}
		}
    }
}
