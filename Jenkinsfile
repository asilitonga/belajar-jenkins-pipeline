pipeline {
    agent {
        node {
            label "linux && java11"
        }
    }

    stages {
        stage('Build') {
            steps {
                echo ("Hello Build")
            }
        }

        stage('Test') {
            steps {
                echo ("Hello Test")
            }
        }

        stage('Deploy') {
            steps {
                echo ("Hello Deploy")
            }
        }
    }

    

    post {
        always {
            echo "Selalu dijalankan"
        }

        success {
            echo "kalau sukses dijalankan"
        }

        failure {
            echo "kalau gagal tidak dijalankan"
        }

        cleanup {
            echo "ini wajib dijalankan"
        }
    }
}