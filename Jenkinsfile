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
                    echo "deployinh image to ec2"
                    def dockerComposeCommand = "docker-compose -f docker-compose.yaml up --detach"
                    sshagent(['ec2-server-key']) {
                        sh "scp docker-compose.yaml ec2-user@54.81.204.137:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@54.81.204.137 ${dockerComposeCommand}"
                    }
                }             
            }
        }
    }
}