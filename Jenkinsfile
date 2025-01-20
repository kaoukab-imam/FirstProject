pipeline {
    agent any
    tools {
        nodejs 'nodejs' // Assurez-vous que "nodejs" correspond à votre configuration dans Jenkins
    }
    environment {
        REPO_URL = 'https://github.com/<votre-utilisateur>/<votrerepo>.git'
        COLLECTION_PATH = 'UserCollection.postman_collection.json'
        ENVIRONMENT_PATH = 'preprod.postman_environment.json'
        DATAVAR_PATH = 'users.json'
    }
    stages {
        stage('Checkout Code') {
            steps {
                // Clone le dépôt Git à partir de la branche principale
                git branch: 'main', url: "${REPO_URL}"
            }
        }
        stage('Install Dependencies') {
            steps {
                // Installer Newman globalement
                bat 'npm install -g newman'
            }
        }
        stage('Run Newman Collection') {
            steps {
                script {
                    // Construire la commande Newman
                    def newmanCommand = "newman run ${COLLECTION_PATH}"
                    
                    // Ajouter les options si les fichiers existent
                    if (fileExists(DATAVAR_PATH)) {
                        newmanCommand += " -d ${DATAVAR_PATH}"
                    }
                    if (fileExists(ENVIRONMENT_PATH)) {
                        newmanCommand += " -e ${ENVIRONMENT_PATH}"
                    }

                    // Exécuter la commande Newman
                    bat newmanCommand
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline terminé, vérifiez les résultats dans la console.'
        }
        success {
            echo 'Tests exécutés avec succès.'
        }
        failure {
            echo 'Erreur dans l’exécution de la pipeline ou des tests Newman.'
        }
    }
}
