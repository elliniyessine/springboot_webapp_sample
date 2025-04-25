pipeline {
    agent any

    triggers {
      // Poll the git repo for new changes every minute
      pollSCM '* * * * *'
    }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('93bb42ec-3f43-4a58-800b-83b1edd6bf8b')
        APP_NAME = "elliniyessine/springboot_webapp_sample"
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'tp2', url: 'https://github.com/elliniyessine/springboot_webapp_sample.git'
            }
        }
        stage('Build the project & run tests') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                sh 'docker build -t $APP_NAME:$BUILD_NUMBER .'
            }
        }

        stage('login to dockerhub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

         stage('push image') {
            steps {
                sh 'docker push $APP_NAME:$BUILD_NUMBER'
            }
        }
    }
}
