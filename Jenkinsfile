pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Upload JAR to Nexus') {
            nexusArtifactUploader artifacts: [
                [
                    artifactId: 'my-app', 
                    classifier: '', 
                    file: 'target/my-app-1.0.0.jar', 
                    type: 'jar'
                ]
            ], 
            credentialsId: '33b4f031-8bab-4f9f-976d-cf771b6035cb', 
            groupId: 'com.mycompany.app', 
            nexusUrl: 'localhost:8081', 
            nexusVersion: 'nexus3', 
            protocol: 'http', 
            repository: 'my-app-release', 
            version: '1.0.0'
        }
    }
}