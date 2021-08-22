pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment{
       ArtifactId = readMavenPom().getArtifactId()
       Version = readMavenPom().getVersion()
       Name = readMavenPom().getName()
       GroupId = readMavenPom().getGroupId()
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
                script {
                def NexusRepo = Version.endsWith("SNAPSHOT") ? "wasimdevopslab-SNAPSHOT" : "wasimdevopslab-RELEASE"
                nexusArtifactUploader artifacts:
                [[artifactId: "${ArtifactId}",
                classifier: '',
                file: "target/${ArtifactId}-${Version}.war",
                type: 'war']],
                credentialsId: '5e6877d7-fef9-4219-8d0f-c0eab8edc50f',
                groupId: "${GroupId}",
                nexusUrl: '10.123.2.68:8081',
                nexusVersion: 'nexus3',
                protocol: 'http',
                repository: "${NexusRepo}",
                version: "${Version}"
                }
            }
        }

        // Stage 4 : Print some information
        stage ('Print Environment variables'){
            steps {
                        echo "Artifact ID is '${ArtifactId}'"
                        echo "Version is '${Version}'"
                        echo "GroupID is '${GroupId}'"
                        echo "Name is '${Name}'"
            }
        }

        // Stage 5 : Deploying to tomcat server
        stage ('deploy'){
            steps {
                echo 'deploying.....'
                sshPublisher(publishers:
                [sshPublisherDesc(
                    configName: 'Ansible_Controller',
                    transfers: [
                        sshTransfer(
                            cleanRemote: false,
                            execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_as_tomcat_user.yml -i /opt/playbooks/hosts',
                            execTimeout: 120000,
                            )
                        ],
                    usePromotionTimestamp: false,
                    useWorkspaceInPromotion: false,
                    verbose: false)
                    ])
            }
        }
        // Stage 6 : download and deploy build artifact to docker
        stage ('deploy to docker'){
            steps {
                echo 'deploying.....'
                sshPublisher(publishers:
                [sshPublisherDesc(
                    configName: 'Ansible_Controller',
                    transfers: [
                        sshTransfer(
                            cleanRemote: false,
                            execCommand: 'ansible-playbook /opt/playbooks/download_and_deploy_docker.yml -i /opt/playbooks/hosts',
                            execTimeout: 120000,
                            )
                        ],
                    usePromotionTimestamp: false,
                    useWorkspaceInPromotion: false,
                    verbose: false)
                    ])
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

