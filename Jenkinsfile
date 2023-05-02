pipeline{

    agent any

    stages{

        stage('Continouse Download'){

            steps{
                git branch: 'main', url: 'https://github.com/clemenrance/momo.git'
            }
        }
        stage('Unit Test'){

            steps{
                sh 'mvn test'
            }
        }
        stage('integration Test'){

            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage('Continouse Build'){

            steps{
                sh 'mvn clean install'
            }
        }
        stage('static Code Analysis'){

            steps{

                script{
                    withSonarQubeEnv(credentialsId: 'sonar-auth'){
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
        stage('quality gate analysis'){

            steps{

                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-auth'
                }
            }
        }
        stage('upload artifact to Nexus'){

            steps{

                script{
                  nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']], credentialsId: 'nexus-link', groupId: 'com.example', nexusUrl: '54.86.229.201:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'repository', version: '1.0.0'  
                }
            }
        }
    }
}