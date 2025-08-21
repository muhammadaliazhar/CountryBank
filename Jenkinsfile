pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/muhammadaliazhar/CountryBank.git'
            }
        }

        stage('OWASP Dependency Check') {
            steps {
                withCredentials([string(credentialsId: 'NVD_API_KEY', variable: 'NVD_KEY')]) {
                    dependencyCheck additionalArguments: "--scan ./ --nvdApiKey=${NVD_KEY} --data /var/lib/jenkins/odc-data", odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                }
            }
        }

        stage('Trivy Scan') {
            steps {
                sh "trivy fs ."
            }
        }

        stage('Build & Deploy') {
            steps {
                sh "whoami"
                sh "docker compose up -d"
            }
        }
    }
}
