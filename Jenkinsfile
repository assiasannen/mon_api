pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'mon-api'
        DOCKER_REGISTRY = 'docker.io' // Si vous utilisez Docker Hub
        DOCKER_REPO = 'assiasannen2105/mon-api' // Votre nom d'utilisateur Docker Hub et le nom de l'image
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                // Clone le dépôt Git où se trouve votre projet
                git 'https://github.com/votre-utilisateur/votre-repository.git'
            }
        }

        stage('Construire l\'image Docker') {
            steps {
                script {
                    // Construire l'image Docker avec le Dockerfile
                    sh 'docker build -t $DOCKER_REPO .'
                }
            }
        }

        stage('Pousser l\'image vers Docker Hub') {
            steps {
                script {
                    // Se connecter à Docker Hub (utilisez vos identifiants Docker Hub)
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    }

                    // Pousser l'image Docker vers Docker Hub
                    sh 'docker push $DOCKER_REPO'
                }
            }
        }

        stage('Déployer l\'image Docker') {
            steps {
                script {
                    // Déployer le conteneur Docker en utilisant l'image poussée
                    sh 'docker run -d -p 5000:5000 $DOCKER_REPO'
                }
            }
        }
    }

    post {
        always {
            // Nettoyer les images Docker après le processus
            sh 'docker system prune -f'
        }
    }
}
