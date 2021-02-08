#!/usr/bin/env groovy
pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }

    stages {
        stage('test') {
            steps {
                withGradle{
                    sh './gradlew clean test'
                }              
            }
            post{
                always{
                    junit 'build/test-results/test/TEST-*.xml'
                }
            }
        }
        stage('test-iT') {
            steps { 
                configFileProvider([configFile(fileId:'hello-grails-gradle.properties', targetLocation: 'gradle.properties')]) {
                    withGradle{
                        sh './gradlew iT'
                    }
                }              
            }
            post{
                always{
                    junit 'build/test-results/integrationTest/TEST-*.xml'
                }
            }
        }
        stage('codenarc') {
            steps {
                withGradle{
                    sh './gradlew codenarcTest'
                }              
            }
            post{
                always{
                    publishHTML (target : [allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'build/reports/codenarc',
                        reportFiles: '*.html',
                        reportName: 'My Reports',
                        reportTitles: 'The Report'])

                }
            }                
        }
	stage('sonarQube') {
            steps { 
                configFileProvider([configFile(fileId: 'hello-spring-testing-gradle.properties', targetLocation: 'gradle.properties')]) {
                    withGradle{
                        withSonarQubeEnv(credentialsId:'9a7e16ef-5513-440a-b2f1-09233ae2e79e', installationName: 'local') { 
                            sh './gradlew clean sonarqube'
                        }
                    }
                }              
            }
        }

    }
}
