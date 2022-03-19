pipeline {
    agent any
    tools {
        maven 'M3'
    }
    stages {
        stage('Git clone'){
            steps{
                git credentialsId: 'GIT_HUB_CREDENTIALS', url: 'https://github.com/saranca14/complete_ci_cd.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the source'
                sh 'mvn clean package'
            }
        }
        stage('Upload war') {
            steps {
                echo 'Upload the war'
                nexusArtifactUploader artifacts: [
                    [   artifactId: 'calc-app',
                        classifier: '', 
                        file: 'target/calc-app-1.0.0.war', 
                        type: 'war'
                    ]
                ], 
                    credentialsId: 'nexus_id', 
                    groupId: 'in.javahome', 
                    nexusUrl: '10.0.1.160', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'calc_app_release', 
                    version: '1.0.0'
            }
        }
    }
}