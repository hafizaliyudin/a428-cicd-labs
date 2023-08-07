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
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                
                // Tahap Persetujuan Manual
                script {
                    def userInput = input(message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)', parameters: [booleanParam(defaultValue: false, description: 'Klik "Proceed" untuk melanjutkan', name: 'proceed')])
                    
                    if (userInput.proceed) {
                        sh './jenkins/scripts/kill.sh'
                    } else {
                        error("Proses dihentikan oleh pengguna.")
                    }
                }
            }
        }
    }
}





