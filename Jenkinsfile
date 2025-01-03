pipeline {
    agent any

    tools {
        maven 'sonarmaven'
    }

    environment {
        MAVEN_PATH = "C:\\apache-maven-3.9.9\\bin"
        SONAR_TOKEN = credentials('assignment-2-maven')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Clean') {
            steps {
                echo 'Cleaning target'
                bat '''
                set PATH=%MAVEN_PATH%;%PATH%
                mvn clean
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Testing'
                bat '''
                set PATH=%MAVEN_PATH%;%PATH%
                mvn test
                '''
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the compiled code...'
                bat '''
                set PATH=%MAVEN_PATH%;%PATH%
                mvn package
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                withSonarQubeEnv('SonarQube') {
                    bat '''
                    set PATH=%MAVEN_PATH%;%PATH%
                    mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=naman-assignment2-maven \
                    -Dsonar.projectName='naman assignment2 maven' \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.token=%SONAR_TOKEN%
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}