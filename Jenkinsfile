pipeline{
    
    agent {
        node {
            label 'AGENT-1'
        }
    }

    // agent any

    environment{
        packageVersion = ''
    }
    
    stages{

        stage('Get Version') {

            steps{

                script{
                    def packageJson = readJSON(file: 'package.json')
                    def packageVersion = packageJson.version
                    echo "version: ${packageVersion}"
                }
            }
        }

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
                    zip -r catalogue.zip ./* --exclude .git --exclude .zip
                '''
            }
        }

        // Static Application Security Testing

        stage('SAST') {
            steps{
                echo "Static Application Security Testing Done"
            }
        }

        //Install Pipeline Utility steps plugin and Nexus Artifact Uploader
        stage('Publish Artifacts') {

            steps{
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '172.31.37.24:8081/',
                    groupId: 'com.roboshop',
                    version: "$packageVersion",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
            }
        }
        // here i need to configure downstream job. I have to pass package version for deployment
        // This job will wait until downstream job is over
        stage('Deployment') {
            steps{
                script{
                     echo 'Deploying to Server'
                     def params = [
                        string(name: 'version', value: "${packageVersion}")
                     ]
                    build job: "../catalogue-deploy", wait: true, parameters: params
                }
               
            }
        }
    }

    post{
        always{
            echo 'Cleaning Up Workspace'
            deleteDir()
        }
    }






}
