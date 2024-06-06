pipeline{
    
    agent {
        node {
            label 'AGENT-1'
        }
    }

    // agent any
    
    stages{

        stage('Install Dependencies') {

            steps{
                sh 'pwd'
                sh 'ls -ltr'
                sh 'npm install'
            }
        }

        stage('Run Unit Tests') {
            steps{
                echo 'Running Unit Tests'
            }
        }

        // sonar-scanner command expects sonar-project.properties should be available
        stage('Sonar Scan') {
            steps{
                echo "Sonar Scan Done"
            }
        }

        stage('Build') {
            steps{
                sh '''
                    pwd
                    ls -ltr
                    zip -r ./* --exclude .git --exclude .zip
                '''
            }
        }

        // Static Application Security Testing

         stage('SAST') {
            steps{
                echo "Static Application Security Testing Done"
            }
        }

        stage('Publish Artifacts') {

            steps{
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '34.229.101.93:8081/',
                    groupId: 'com.roboshop',
                    version: '1.0.0',
                    repository: 'catalogue',
                    credentialsId: 'CredentialsId',
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
            }
        }

        stage('Deployment') {
            steps{
                echo 'Deploying to Server'
            }
        }
    }

    post{
        always{
            echo 'Cleaning Up Workspace'
            //deleteDir()
        }
    }






}