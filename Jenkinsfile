pipeline {
    agent {
        dockerfile {
            dir 'agent'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Analyse SCA (Dépendances)') {
            steps {
                sh 'npm install'
                sh 'npm audit --audit-level=high'
            }
        }
        stage('Analyse SAST (SonarQube)') {
            environment {
                scannerHome = tool 'SonarScanner'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage('Vérification Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Build & Scan Image Docker') {
            steps {
                script {
                    def dockerImage = docker.build("mon-app-pipeline:${env.BUILD_ID}")
                    sh "trivy image --exit-code 1 --severity CRITICAL mon-app-pipeline:${env.BUILD_ID}"
                }
            }
        }
    }
}
