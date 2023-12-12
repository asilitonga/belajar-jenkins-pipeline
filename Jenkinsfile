pipeline {
    agent none
//agent adalah node/vm. kita jalankan script ini di node yang mana
//kita bisa atur agent/nodenya di: manage jenkins > node > buat node baru. kita bisa tambahkan banyak node
//terus kita bisa tentukan setiap stage dijalankan di AGENT yang mana dengan syarat diurutan awal kita tentukan: AGENT NONE
//nanti baru disetiap stages stage kita bisa tambahkan dibawahnya agent
//jadi urutannya nanti, setiap stage harus ada agent, dan tentukan stagenya dijalankan di agent yg mana

//didalam stage kita juga bisa membuat stages dengan catatan TIDAK BOLEH ADA STEPS didalam stage
//urutannya wajib: stage dibawahnya stages, stages dibawahnya stage, stage dibawahnya steps, dan steps dibawahnya command

//urutan pipeline groovy, yaitu:
//level 1: pipeline
//level 2, ada 3 jenis yaitu: 1. kode agent (WAJIB), 2. kode fungsi, dan 3. kode deploy
//level 2 kode agent seperti: dibawah pipeline ada AGENT NONE (WAJIB)
//
//level 2 kode fungsi seperti: 
//fungsi parameters, fungsi triggers/cronjob, fungsi options, dan fungsi post
//
//level 2 kode deploy seperti: stages stage agent, DAN stages stage agent stages stage (fungsi ini tanpa steps ya)

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

    //menjalankan pipeline dengan 1 agent saja dengan jeda 50 detik
    options {
        disableConcurrentBuilds()
        timeout(time: 60, unit: 'SECONDS')
    }

//buat parameter choice
    stages {

        stage ("Membuat Fungsi Matrix Paralel Bersamaan") {
            matrix {
                axes {
                    //axis 1
                    axis {
                        name "OS"
                        values "windows", "linux", "mac"
                    }
                    //axis 2
                    axis {
                        name "TypeOS"
                        values "32", "64"
                    }
                }
            
                excludes {
                    exclude {
                        axis {
                            name "OS"
                            values "mac"
                        }
                        axis {
                            name "TypeOS"
                            values "32"
                        }
                    }
                }
                stages {
                    stage ("Panggil Fungsi Matrix Paralelnya") {
                        agent {
                            node {
                                label "linux && java11"
                            }
                        }
                        steps {
                            echo ("Running OS ${OS} ${TypeOS}")
                        }  
                    }
                }
            }
        }
//tutup

        stage ("Preparation") {
            agent {
                label "linux && java11"
            }
        
            stages {
                stage ("Parameter test 1") {
                    steps {
                        echo ("ini adalah test 1")
                    }
                }
                stage ("Parameter test 2") {
                    steps {
                        echo ("ini adalah test 2")
                    }
                }
            }        
        }
//tutup

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
//tutup
              
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
                withCredentials([usernamePassword(
                    credentialsId: "secret-asilitonga"
                    usernameVariable: "USER"
                    passwordVariable: "PASSWORD" 
                )]) {
                    sh ('echo "Release it with -u $USER -p $PASSWORD" > "release.txt"')
                }
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