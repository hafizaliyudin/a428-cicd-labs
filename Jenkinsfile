pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Manual Approval') {
            steps {
                script {
                    def userInput = input(
                        id: 'userInput',
                        message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk mengakhiri)',
                        parameters: [string(defaultValue: 'Proceed', description: 'Klik "Proceed" untuk mengakhiri', name: 'Proceed')]
                    )
                    if (userInput == 'Proceed') {
                        echo 'Melanjutkan ke tahap Deploy...'
                    } else {
                        error('Persetujuan tidak diberikan. Proses dihentikan.')
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
        stage('Finalize') {
            steps {
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
