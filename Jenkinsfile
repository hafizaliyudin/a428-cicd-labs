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
Dalam kode di atas, saya telah menambahkan bagian skrip persetujuan manual menggunakan blok script. Tahap persetujuan manual akan menampilkan kotak dialog di antarmuka Jenkins dengan pilihan "Proceed" atau "Abort". Jika pengguna memilih "Proceed", maka skrip ./jenkins/scripts/kill.sh akan dijalankan untuk mengakhiri proses. Jika pengguna memilih "Abort", alur kerja akan dihentikan dengan pesan kesalahan.

Pastikan bahwa skrip kill.sh benar-benar mengakhiri proses yang diperlukan untuk aplikasi React Anda. Selain itu, pastikan bahwa skrip lainnya juga berjalan sesuai kebutuhan Anda.

Ingat untuk selalu menguji alur kerja Anda setelah membuat perubahan seperti ini untuk memastikan semuanya berjalan dengan baik.






