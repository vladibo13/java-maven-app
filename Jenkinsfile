#!/usr/bin/env groovy
library identifier: 'jenkins-shared-lib@main', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/vladibo13/jenkins-shared-libary.git',
     credentialsId: 'github-secret'
    ]
)

pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }

    environment {
        IMAGE_NAME = 'vladibo/java-maven-app:1.2'
    }


    stages{
        stage("build app") {
            steps {
                script {
                  echo "testing the app..."
                  buildJar()
                }
            }
        }

        stage("build image") {
            steps {
                script {
                    echo "building docker image"
                    buildImage(env.IMAGE_NAME)
                    dockerLogin()
                    dockerPush(env.IMAGE_NAME)
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    def dockerCmd = "docker run -p 3080:3080 -d vladibo/react-node-example:1.1"
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@54.81.204.137 ${dockerCmd}"
                    }
                }             
            }
        }
    }
}