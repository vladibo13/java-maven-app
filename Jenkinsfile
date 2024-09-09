pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }
    environment {
        NEW_VERSION = '1.3.1'
    }
    stages{
        stage("test") {
            steps {
                script {
                    echo "Testing the application..."
                    echo "Executing the pipeline for branch $BRANCH_NAME"
                }
            }
        }
        stage("build jar") {
            when{
                expression {
                    BRANCH_NAME == "main"
                }
            }
            steps {
                script {
                    echo "buildeing the app..."
                    // sh 'mvn package'
                }
            }
        }

        // stage("build image") {
        //     steps {
        //         script {
        //             echo "buildeing the docker image"
        //             withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
        //                 sh 'docker build -t vladibo/demo-app:jma-1.2 .'
        //                 sh 'echo $PASS | docker login -u $USER --password-stdin'
        //                 sh 'docker push vladibo/demo-app:jma-1.2'
        //             }
        //         }
        //     }
        // }

        stage("deploy") {
            when {
                expression {
                    BRANCH_NAME == "main"
                }
            }

            steps {
                echo "deploying the app..."
            }
        }
    }
}