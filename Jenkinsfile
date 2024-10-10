#!/usr/bin/env groovy
@Library('jenkins-shared-library')
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

        stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
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
                    buildImage "vladibo/demo-app:${IMAGE_NAME}"
                    dockerLogin()
                    dockerPush "vladibo/demo-app:${IMAGE_NAME}"
                }
            }
        }

        stage("deploy") {
            steps {
                echo "deploying the app updated 324234..."
            }
        }

        stage("commit version update") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-secret', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    // git config
                    sh 'git config --global user.email "jenkins@example.com"'
                    sh 'git config --global user.name "jenkins"'

                    sh 'git status'
                    sh 'git branch'
                    sh 'git config --list'
                    
                    sh "git remote set-url origin https://${USER}:${PASS}@github.com/vladibo13/java-maven-app.git"
                    sh 'git add .'
                    sh 'git commit -m "version update"'
                    sh 'git push origin HEAD:jenkins-shared-lib'
                }
            }
        }
    }
}