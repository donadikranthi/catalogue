pipeline {
    // These are pre-build sections
    agent any 
    
    environment {
        COURSE = "Jenkins"
        appVersion = ""
        ACC_ID = "327425719057"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options {
        timeout(time: 10, unit: 'MINUTES') 
        disableConcurrentBuilds()
    }
    // This is build section
    stages {
        stage('Read Version') {
            steps {
                script{
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                    echo "app version: ${appVersion}"
                }
            }0
        }
        stage('Install Dependencies') {
            steps {
                script{
                    sh """
                        npm install
                    """
                }
            }
        }
         stage('Build Image') {
            steps {
                script{
                    withAWS(region:'us-east-1',credentials:'aws-creds') {
                        sh """
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS
                         --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                        docker build ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}-${COMPONENT}:${appVersion} .
                        docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}-${COMPONENT}:${appVersion} 

                        """
                    }
                }
            }
        }
    }
    post {
        always {
        echo 'I will always say Hello again!'
        cleanWs()
        }
        success {
            echo 'pipeline is successful'
        }
        failure {
            echo 'pipeline has failed'
        }
        aborted {
             echo 'pipeline is aborted'
        }
    }
}