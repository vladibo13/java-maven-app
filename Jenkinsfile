#!/usr/bin/env groovy
@Library('jenkins-shared-libary')
def gv 


pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }

    stages{
        stage("init") {
            steps {
                script {
                  gv = load "script.groovy"
                }
            }
        }

        stage("build jar") {
            steps {
                script {
                   buildJar()
                }
            }
        }

        stage("build and push image") {
            steps {
                script {
                    buildImage "vladibo/demo-app:jma-1.4"
                    dockerLogin()
                    dockerPush "vladibo/demo-app:jma-1.4"
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