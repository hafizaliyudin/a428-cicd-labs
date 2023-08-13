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
        stage('Deploy - Pause for Approval') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
        stage('Manual Approval') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                input message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk mengakhiri)'
            }
        }
        stage('Deploy - Finalize') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
