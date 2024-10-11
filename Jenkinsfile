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
                sshagent(['ec2-server-key']) {
                }
            }
        }

    }
}