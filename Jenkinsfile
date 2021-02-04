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
                withGradle{
                    sh './gradlew -Dgeb.env=firefoxHeadless iT'
                }              
            }
            post{
                always{
                    junit 'build/test-results/integrationTest/TEST-*.xml'
                }
            }
        }
        stage('test-iT') {
            steps {
                withGradle{
                    sh './gradlew clean -Dgeb.env=firefoxHeadless iT'
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
        }
    }
}
