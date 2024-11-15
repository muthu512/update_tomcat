pipeline {
    agent any

    environment {
        PROJECT_DIR = "C:\\Users\\Dell-Lap\\Downloads\\login360ui"
        TOMCAT_DIR = "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\work"
        APP_NAME = "login"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git credentialsId: 'muthu512', url: 'https://github.com/muthu512/update_tomcat.git', branch: 'master'
            }
        }

        stage('Check Node and npm Versions') {
            steps {
                script {
                    bat 'node -v'
                    bat 'npm -v'
                }
            }
        }

        stage('Check Environment Variables') {
            steps {
                script {
                    bat 'echo %PATH%'
                }
            }
        }

        stage('Prepare Project') {
            steps {
                script {
                    dir(PROJECT_DIR) {
                        bat 'if exist "package.json" (echo package.json exists) else (echo package.json not found && exit 1)'
                    }
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    dir(PROJECT_DIR) {
                        bat 'npm install'
                    }
                }
            }
        }

        stage('Optimized Build React App') {
            steps {
                script {
                    dir(PROJECT_DIR) {
                        // Set NODE_OPTIONS to use legacy OpenSSL provider for the build
                        bat 'set NODE_OPTIONS=--openssl-legacy-provider && npm run build'
                    }
                }
            }
        }

        stage('Deploy to Tomcat') { // Fixed typo here
            steps {
                script {
                    bat "if not exist \"${PROJECT_DIR}\\build\" (echo Build directory does not exist && exit 1)"
                    bat "dir \"${PROJECT_DIR}\\build\""
                    bat "if not exist \"${PROJECT_DIR}\\build\\*\" (echo No files in build directory && exit 1)"
                    bat "if not exist \"${TOMCAT_DIR}\\${APP_NAME}\" mkdir \"${TOMCAT_DIR}\\${APP_NAME}\""
                    bat "xcopy /S /I /Y \"${PROJECT_DIR}\\build\\*\" \"${TOMCAT_DIR}\\${APP_NAME}\\\""
                    bat "dir \"${TOMCAT_DIR}\\${APP_NAME}\""
                }
            }
        }
    }

    post {
        always {
            echo 'This will always run after the pipeline finishes'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        unstable {
            echo 'Pipeline unstable!'
        }
    }
}
