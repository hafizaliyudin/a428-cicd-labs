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
                echo 'Installing Cloudflare Wrangler'
                sh 'yarn add wrangler'
                echo 'Deploying to cloudflare pages'
                sh 'CLOUDFLARE_ACCOUNT_ID=RAHASIA CLOUDFLARE_API_TOKEN=RAHASIA npx wrangler pages publish build --project-name=RAHASIA'
                sh './jenkins/scripts/deliver.sh'
                sh 'sleep 60'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}