pipeline {
    agent any
    tools {maven 'localMaven'}
    stages {
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now archiving....'
                    archiveArtifacts artifacts: '**/target/*.war'
                    
                }
            }
        }
        stage ('Deploy to staging') {
            steps {
                build job: 'deploy-to-staging'
            }
        }
        stage('Deploy to production') {
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve deploy to production?', submitter: admin
                }

                build job:deploy-to-prod
            }
            post{
                success {
                    echo 'Succesfully deployed to production'
                }
                failure {
                    echo 'Deployment failed'
                }

            }
        }
    }
}