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
    // options {
    //     timeout(time: 10, unit: 'MINUTES') 
    //     disableConcurrentBuilds()
    // }
    // This is build section
     // This is build section
    stages {
        stage('Read Version') {
            steps {
                script{
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                    echo "app version: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script{
                    sh """
                        npm install python3 -m pip install -r requirements.txt
                    """
                }
            }
        }
        stage('Unit Test') {
            steps {
                script{
                    sh """
                        npm test
                    """
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