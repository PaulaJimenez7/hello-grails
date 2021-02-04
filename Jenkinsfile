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
                configFileProvider([configFile('hello-grails-gradle.properties', variable: 'systemProp.geb.env'))]) {
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
    }
}
