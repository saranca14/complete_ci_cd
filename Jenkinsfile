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
                script{
                def mavenPom = readMavenPom file: 'pom.xml'
                def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "calc_app_snapshot" : "calc_app_release"
                echo 'Upload the war to nexus'
                nexusArtifactUploader artifacts: [
                    [   artifactId: 'calc-app',
                        classifier: '', 
                        file: "target/calc-app-${mavenPom.version}.war", 
                        type: 'war'
                    ]
                ], 
                    credentialsId: 'nexus_id', 
                    groupId: 'in.javahome', 
                    nexusUrl: '3.109.186.152:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexusRepoName, 
                    version: "${mavenPom.version}"
                }
            }
        }
    }
}