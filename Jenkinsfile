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