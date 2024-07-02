pipeline {
    agent any
    tools {
        gradle 'Gradle 8.5'
    }
    environment {
        VERSION_NUMBER = '1.0'
    }
    stages {
        stage('Clone repository') {
            steps {
                echo 'Cloning repository'
                git 'https://github.com/brianmarete/java-todo.git'
            }
        }
        stage('Build ') {
            steps {
                echo "Build number ${BUILD_NUMBER}"
                sh 'gradle build'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing the project'
                sh 'gradle test'
            }
        }
        stage('Deploy to Heroku') {
            steps {
                withCredentials([string(credentialsId: 'HEROKU_API_KEY', variable: 'HEROKU_API_KEY')]) {
                    sh '''
                        git push https://heroku:$HEROKU_API_KEY@git.heroku.com/jeans_heroku.git HEAD:main
                    '''
                }
            }
        }
    }
    post {
        success {
            slackSend color: "good", message: "Build #${BUILD_NUMBER} ran successfully"
        }
        
        failure {
            slackSend color: "danger", message: "Build #${BUILD_NUMBER} failed"
        }
    }
}