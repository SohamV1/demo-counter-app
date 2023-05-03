pipeline{
    agent any
    stages{
        stage("Git Checkout"){
            steps{
                script{
                    git 'https://github.com/SohamV1/demo-counter-app.git'
                }
            }
        }
        stage(" Unit testing"){
            steps{
                script{
                    sh 'mvn test'
                }
            }
        }
        stage("Integration testing"){
            steps{
                script{
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage("maven build"){
            steps{
                script{
                    sh 'mvn clean install'
                }
            }
        }
        stage("Static Code Analysis"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonarqube') {
                      sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
        // stage("Quality Gate status"){
        //     steps{
        //         script{

        //             waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube'
        //         }
        //     }
        // }
        stage("Upload jar file to nexus"){
            steps{
                script{
                    def readPomVersion = readMavenPom file: 'pom.xml'

                    def nexusRepo = readMavenPom.version.endsWith("SNAPSHOT") ? "demoapp-snapshot" : "demoapp-release"
                    
                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'spring-boot-starter-parent', 
                            classifier: '', file: 'target/Uber.jar', 
                            type: 'jar'
                            ]
                    ], 
                    credentialsId: '4c162fa8-e0f0-49b6-9992-a57c1030823b', 
                    groupId: 'com.example', 
                    nexusUrl: '23.20.140.89:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexusRepo , 
                    version: "${readPomVersion.version}"
                }
            }
        }
    }
    post{
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}