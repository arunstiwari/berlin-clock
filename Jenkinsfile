pipeline {
    agent any

    tools {
        jdk 'jdk8'
        maven 'maven3'
    }

    stages {
        stage('install and sonar parallel') {
            steps {
                parallel(install: {
                    sh "mvn -U clean test cobertura:cobertura -Dcobertura.report.format=xml"
                }, sonar: {
                    sh "mvn package"
                })
            }
            post {
                always {
                    junit '**/target/*-reports/TEST-*.xml'
                    step([$class: 'CoberturaPublisher', coberturaReportFile: 'target/site/cobertura/coverage.xml'])
                }
            }
        }
        stage('SonarQube analysis') {
            // requires SonarQube Scanner 2.8+
             def scannerHome = tool 'ADOP SonarScanner';
             withSonarQubeEnv('ADOP Sonar') {
                 sh "${scannerHome}/bin/sonar-scanner"
             }
        }
    }
}