pipeline {
    agent none
//kalau mau buat node agent dijalankan di vm masing" gunakan: agent none diawalnya
//terus nanti tinggal panggil aja masing-masing agentnya per stage

    parameters {
        string(name: "NAME", defaultValue: "Guest", description: "ini paramater string")
        text(name: "DESCRIPTION", defaultValue: "Guest", description: "ini paramater text")
        booleanParam(name: "DEPLOY", defaultValue: false, description: "ini paramater booleanparam yes or no")
        choice(name: "SOCIAL_MEDIA", choices: ['facebook', 'instagram', 'telegram'], description: "ini paramater choice/combobox")
        password(name: "PASSWORDNYA", defaultValue: "", description: "ini paramater password key")
    }

    //menjalankan pipeline dengan waktu yang ditentukan
    //triggers {
        //pollSCM("*/5 * * * *")
    //}

    //menjalankan pipeline dengan 1 agent saja dengan jeda 40 detik
    options {
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'MINUTES')
    }

//buat parameter choice
    stages {
        
        //done        
        stage("Parameter") {
            agent {
                node {
                    label "linux && java11"
                }
            }
        
            steps {
                echo "string:      ${params.NAME}"
                echo "text:        ${params.DESCRIPTION}"
                echo "boolean:     ${params.DEPLOY}"
                echo "choice:      ${params.SOCIAL_MEDIA}"
                echo "password:    ${params.PASSWORDNYA}"
            }
        }

//1
        stage("Prepare") {
            //buat credential dgn env
            environment {
            //buat fungsinya dulu diawal, baru dibawahnya tinggal panggil fungsinya
                APP = credentials("secret-asilitonga")
        }            
            agent {
                node {
                    label "linux && java11"
                }
            }

            steps {
                echo ("Start Job: ${env.JOB_NAME}")
                echo ("Start Build: ${env.BUILD_NUMBER}")
                echo ("Branch Name: ${env.BRANCH_NAME}")
                echo ("Username: ${APP_USR}")
                sh('echo "Passwordnya: $APP_PSW" > "rahasia.txt"')
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
            input {
                message "Can we deploy?"
                ok "yes, of course"
                submitter "asilitonga, papajenkins"
            
                parameters {
                    choice(name: "pilihan", choices:['DEV', 'QA', 'PROD'], description: "deploy gak?")
                }
            }

            agent {
                node {
                    label "linux && java11"
                }
            }

            steps {
                echo ("Deploy ke ${pilihan}")
            }
        }

    
//5
        stage("Release") {
            when {
                expression {
                    return params.DEPLOY
                }
            }

            agent {
                node {
                    label "linux && java11"
                }
            }
            
            steps {
                echo "jika bernilai TRUE/koddingnya bisa jalan artinya scriptnya OK"
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