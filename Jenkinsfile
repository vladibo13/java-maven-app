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

        stage("build image") {
            steps {
                script {
                    buildImage()
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