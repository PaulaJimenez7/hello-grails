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
                    jacoco(execPattern: 'build/jacoco/test.exec')
                }
            }
        }
        stage('test-iT') {
            steps {
                withGradle{
                    sh './gradlew clean -Dgeb.env=firefox iT'
                }              
            }
            post{
                always{
                    junit 'build/test-results/integrationTest/TEST-*.xml'
                    jacoco(execPattern: 'build/jacoco/test.exec')
                }
            }
        }
    }
}
