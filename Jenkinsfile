pipeline {
    agent {
        node {
            label "linux && java11"
        }
    }

    stages {
        stage('Build') {
            steps {
                echo ("Start Build")
                sh ("./mvnw clean compile test-compile")
                echo ("Finish Build")
            }
        }

        stage('Test') {
            steps {
                echo ("Start Test")
                sh ("./mvnw test")
                echo ("Finish Test")
            }
        }

        stage('Deploy') {
            steps {
                echo ("Start Deploy")
                sh ("./mvnw clean compile test-compile")
                echo ("Finish Deploy")
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