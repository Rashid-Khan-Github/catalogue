pipeline{
    
    // agent {
    //     node {
    //         label 'AGENT-1'
    //     }
    // }

    agent any

    stages{

        stage('Install Dependencies') {

            steps{
                sh 'pwd'
                sh 'ls -ltr'
                sh 'npm install'
            }
        }

        stage('Build') {

            steps{
                sh '''
                    pwd
                    ls -ltr
                    zip -r ./*
                '''
            }
        }

        stage('Run Unit Tests') {
            steps{
                echo 'Running Unit Tests'
            }
        }
        // sonar-scanner command expects sonar-project.properties should be available
        // stage('Sonar Scan') {
        //     steps{
        //         sh '''
        //             ls -ltr'
        //             cd catalogue
        //             sonar-scanner
        //         '''
        //     }
        // }

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