pipeline {
    agent any

    environment {
        APP_NAME = "secureapp"
        ZIP_NAME = "${APP_NAME}-${BUILD_NUMBER}.zip"
        HASH_FILE = "hash-${BUILD_NUMBER}.txt"
        LATEST_ZIP = "${APP_NAME}-latest.zip"
        LATEST_HASH = "hash-latest.txt"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main2', url: 'https://github.com/takenseyi/secureapp.git'
            }
        }

        stage('Package App') {
            steps {
                sh """
                    zip -r ${ZIP_NAME} app -x "app/.git/*"
                    sha256sum ${ZIP_NAME} > ${HASH_FILE}
                """
            }
        }

        stage('Prepare Latest Artifacts') {
            steps {
                sh """
                    cp ${ZIP_NAME} ${LATEST_ZIP}
                    cp ${HASH_FILE} ${LATEST_HASH}
                """
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: "${ZIP_NAME}, ${HASH_FILE}, ${LATEST_ZIP}, ${LATEST_HASH}", fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Artifacts packaged and archived successfully."
        }
        failure {
            echo "Build failed. Check logs for details."
        }
    }
}
