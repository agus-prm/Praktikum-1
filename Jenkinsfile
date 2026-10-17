pipeline {
    agent any

    environment {
        // GANTI sesuai Docker Hub kamu
        DOCKER_IMAGE = 'usernamedockerhub/praktikum-1'
        // ID credentials (Username with password) di Manage Jenkins > Credentials
        DOCKER_CREDS = 'dockerhub-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Melakukan checkout dari SCM...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Mulai build aplikasi'
                bat 'echo Build aplikasi berjalan'
                // Contoh kalau project Node.js:
                // bat 'npm install'
                // bat 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker --version'
                bat 'docker build -t %DOCKER_IMAGE%:%BUILD_NUMBER% -t %DOCKER_IMAGE%:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDS}",
                                                  usernameVariable: 'DOCKER_USER',
                                                  passwordVariable: 'DOCKER_PASS')]) {
                    // Jangan beri spasi sebelum tanda | agar password tidak ikut spasi
                    bat 'echo %DOCKER_PASS%| docker login -u %DOCKER_USER% --password-stdin'
                    bat 'docker push %DOCKER_IMAGE%:%BUILD_NUMBER%'
                    bat 'docker push %DOCKER_IMAGE%:latest'
                }
            }
        }
    }

    post {
        always {
            bat 'docker logout'
            echo 'Selesai build 🏁'
        }
    }
}
