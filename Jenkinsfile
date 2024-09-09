pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }
    environment {
        NEW_VERSION = '1.3.0'
    }
    stages{
        stage("build jar") {
            steps {
                script {
                    echo "buildeing the app..."
                    sh 'mvn package'
                }
            }
        }

        stage("build image") {
            steps {
                script {
                    echo "buildeing the docker image"
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'docker build -t vladibo/demo-app:jma-1.2 .'
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh 'docker push vladibo/demo-app:jma-1.2'
                    }
                }
            }
        }

        stage("deploy") {
            steps {
                echo "deploying the app..."
            }
        }
    }
}