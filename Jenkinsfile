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

        stage('Trigger Ansible Deployment') {
            steps {
                sh 'ansible-playbook -i inventory.ini site.yml'
            }
        }
    }

    post {
        success {
            echo "Artifacts packaged and deployed successfully."
        }
        failure {
            echo "Build failed. Check logs for details."
        }
    }
}
