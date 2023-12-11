pipeline {
    agent {
        node {
            label "linux && java11"
        }
    }

    stages {
        stage("Build") {
            steps {

                script {
                    for (int i = 0; i < 10 ; i++) {
                        echo("Script ${i}")
                    }
                } 

                echo ("Start Build")
                sh ("./mvnw clean compile test-compile")
                echo ("Finish Build")
            }
        }

        stage("Test") {
            steps {
                script {
                    def data = [
                        "firstName": "Andreas"
                        "lastName": "Silitonga"
                    ]
                    writeJSON(file: "data.json", json: data)
                }

                echo ("Start Test")
                sh ("./mvnw test")
                echo ("Finish Test")
            }
        }

        stage("Deploy") {
            steps {
                echo ("Start Deploy")
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