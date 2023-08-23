pipeline {
    agent {
        docker {
            image 'node:16.13.0'
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
                sh 'CLOUDFLARE_ACCOUNT_ID=466cc72554c0f657714ff53e3dc0e0a4 CLOUDFLARE_API_TOKEN=ojRlpfhN_do2vzIM0Sp1-58xKsXbtbBxMdJLvCcM npx wrangler pages publish build --project-name=a428-cicd-labs'
                sh './jenkins/scripts/deliver.sh'
                sh 'sleep 60'   
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}