pipeline {
    agent none

    stages {
//1
        stage("Prepare") {
            agent {
                node {
                    label "linux && java11"
                }
            }

            steps {
                echo ("Menggunakan brance: ${BRANCE_NAME}")
                echo ("Nama jobnya: ${JOB_NAME}")
                echo ("BUILD NUMBER ${BUILD_NUMBER}")
            }
        }


//2
        stage("Build") {
            agent {
                node {
                    label "linux && java11"
                }
            }

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

//3
        stage("Test") {
            agent {
                node {
                    label "linux && java11"
                }
            }

            steps {
                script {
                def data = [
                    "firstName": "Andreas",
                    "lastName": "Silitonga"
                ]
                writeJSON(file: "data.json", json: data)
                }
                echo ("Start Test")
                sh ("./mvnw test")
                echo ("Finish Test")
            }
        }

//4
        stage("Deploy") {
            agent {
                node {
                    label "linux && java11"
                }
            }

            steps {
                echo ("Start Deploy")
                echo ("Finish Deploy")
            }
        }
    }

//ini adalah script fokus pada fungsi
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