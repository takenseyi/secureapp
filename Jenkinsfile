pipeline {
    agent any

    environment {
        APP_NAME = "secureapp"
        ZIP_NAME = "${APP_NAME}.zip"
        HASH_FILE = "hash.txt"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main1', url: 'https://github.com/takenseyi/secureapp.git'
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

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: "${ZIP_NAME}, ${HASH_FILE}", fingerprint: true
            }
        }

        // 🔍 NEW STAGE using the RHEL 10 Agent
        stage('Check RHEL10 Agent') {
            agent { label 'rhel10' }
            steps {
                sh 'whoami; hostname; ansible --version'
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
