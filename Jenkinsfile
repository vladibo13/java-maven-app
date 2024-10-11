#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }

    stages{
        stage("test") {
            steps {
                script {
                  echo "testing the app"
                }
            }
        }

        stage("build") {
            steps {
                script {
                   echo "building the app"
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