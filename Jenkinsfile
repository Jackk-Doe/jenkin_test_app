pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/Jackk-Doe/jenkin_test_app.git'
        DOCKER_HUB_REPO = 'jackkdoe/jenkin_test_app'
        DOCKERHUB_CREDS = credentials('dockerhub-creds')
        SSH_CREDS = credentials('vm-ssh-password')
        DEPLOY_HOST = '143.198.199.4'             // ⬅️ replace with your VM IP or domain
        DEPLOY_PATH = '/jenkin_test_app'           // ⬅️ folder containing docker-compose.yml
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-creds', url: "${GITHUB_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_HUB_REPO}:latest ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh """
                echo ${DOCKERHUB_CREDS_PSW} | docker login -u ${DOCKERHUB_CREDS_USR} --password-stdin
                docker push ${DOCKER_HUB_REPO}:latest
                """
            }
        }

        stage('Deploy to VM') {
            steps {
                script {
                    // Use sshpass to provide password non-interactively
                    sh """
                    apt-get update -y && apt-get install -y sshpass || true

                    sshpass -p '${SSH_CREDS_PSW}' ssh -o StrictHostKeyChecking=no ${SSH_CREDS_USR}@${DEPLOY_HOST} '
                      cd ${DEPLOY_PATH} &&
                      docker compose pull &&
                      docker compose up -d
                    '
                    """
                }
            }
        }
    }
}
