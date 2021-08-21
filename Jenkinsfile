pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }
        // Stage 3 : Publish the artifact to nexus
        stage ('Publish to nexus'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/VinayDevOpsLab-0.0.4-SNAPSHOT.war', type: 'war']], credentialsId: '5e6877d7-fef9-4219-8d0f-c0eab8edc50f', groupId: 'com.vinaysdevopslab', nexusUrl: '10.123.2.68:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'wasimdevopslab-SNAPSHOT', version: '0.0.4-SNAPSHOT'
            }
        }
        // Stage3 : Deploying
        stage ('deploy'){
            steps {
                echo 'deploying.....'
            }
        }
/*
        // Stage3 : Publish the source code to Sonarqube
        stage ('Sonarqube Analysis'){
            steps {
                echo ' Source code published to Sonarqube for SCA......'
                withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
                     sh 'mvn sonar:sonar'
                }

            }
*/
        }

        
        
    }

