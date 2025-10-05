pipeline {
    agent {
        dockerfile {
            dir 'agent'
            additionalBuildArgs '--no-cache'
            // Ajout de cette ligne pour connecter l'agent au bon réseau
            args '--network mon-app-pipeline_default' 
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
                // Demande à Jenkins de fournir l'outil 'SonarScanner'
                scannerHome = tool 'SonarScanner'
            }
            steps {
                // Utilise la connexion 'SonarQube' configurée dans Jenkins
                withSonarQubeEnv('SonarQube') {
                    // Exécute le scanner fourni par Jenkins
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
