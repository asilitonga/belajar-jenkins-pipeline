pipeline {
    agent {
        node {
            label "linux && java11"
        }
    }

    stages {
        stage('Hello') {
            steps {
                echo ("Perkenalkan nama saya adalah Andreas Silitonga")
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