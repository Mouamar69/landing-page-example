pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/fredericEducentre/landing-page-example.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Build en cours..."'
                sh 'ls -la'
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Tests en cours..."'
            }
        }

        stage('Deploy') {
            steps {

                sshagent(['alwaysdata-ssh']) { 
                    script {
                        def USER = 'mouamar'
                        def REMOTE_DIR = '/home/mouamar/www/landing-page-example'

                        sh """
                            echo "Connexion au serveur Alwaysdata avec clé SSH..."

                            ssh -o StrictHostKeyChecking=no ${USER}@ssh-${USER}.alwaysdata.net \
                            "mkdir -p ${REMOTE_DIR} && rm -rf ${REMOTE_DIR}/*"

                            echo "Déploiement des fichiers..."
                            scp -o StrictHostKeyChecking=no -r ./* ${USER}@ssh-${USER}.alwaysdata.net:${REMOTE_DIR}/

                            echo "Déploiement terminé."
                        """
                    }
                }

            }
        }
    }

    post {
        success {
            echo 'Déploiement réussi sur Alwaysdata avec clé SSH !'
        }
        failure {
            echo 'Échec du déploiement !'
        }
    }
}
