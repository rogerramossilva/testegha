pipeline {
    agent any

    environment {
        TAG = "${env.BUILD_NUMBER}"
        REGISTRY = "${env.REGISTRY_URL}"

        IMAGE_WEB   = "${env.IMAGE_WEB}:${TAG}"
        IMAGE_DB    = "${env.IMAGE_DB}:${TAG}"
        IMAGE_NGINX = "${env.IMAGE_NGINX}:${TAG}"

        COMPOSE_FILE = "${env.COMPOSE_FILE}"

        PROD_HOST = "${env.PROD_HOST}"
        PROD_PATH = "${env.PROD_PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                sh 'echo "Checking out repository..."'
                checkout scm
                slackSend channel: '#ci', message: "Checkout iniciado", tokenCredentialId: 'slack-token'
            }
        }

        stage('Build Images') {
            steps {
                slackSend channel: '#ci', message: "Build das imagens iniciado", tokenCredentialId: 'slack-token'

                script {
                    docker.withRegistry("https://${REGISTRY}", 'dockerhub') {
                        docker.build(IMAGE_WEB,   "-f Dockerfileweb .").push()
                        docker.build(IMAGE_DB,    "-f Dockerfiledb .").push()
                        docker.build(IMAGE_NGINX, "-f Dockerfilenginx .").push()
                    }
                }
            }
        }

        stage('Security Scan (Trivy)') {
            steps {
                slackSend channel: '#ci', message: "Rodando Trivy Scan...", tokenCredentialId: 'slack-token'

                sh """
                    trivy image --severity HIGH,CRITICAL --exit-code 0 ${IMAGE_WEB}
                    trivy image --severity HIGH,CRITICAL --exit-code 0 ${IMAGE_DB}
                    trivy image --severity HIGH,CRITICAL --exit-code 0 ${IMAGE_NGINX}
                """
            }
        }

        stage('Test in Containers') {
            steps {
                slackSend channel: '#ci', message: "Subindo containers para testes...", tokenCredentialId: 'slack-token'

                sh """
                    docker rm -f web db nginx || true
                    docker compose -f ${COMPOSE_FILE} up -d

                    # Teste básico do serviço web
                    sleep 5
                    curl -I http://localhost || exit 1

                    docker compose -f ${COMPOSE_FILE} down
                """
            }
        }

        stage('Deploy to Production') {
            steps {
                slackSend channel: '#ci', message: "Deploy em produção iniciado...", tokenCredentialId: 'slack-token'

                sshagent(credentials: ['ssh-prod']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${PROD_HOST} '
                            cd ${PROD_PATH} &&
                            docker compose pull &&
                            docker compose up -d
                        '
                    """
                }
            }
        }
    }

    post {
        success {
            slackSend channel: '#ci', message: "Pipeline finalizado com sucesso! Build #${BUILD_NUMBER}", tokenCredentialId: 'slack-token'
        }
        failure {
            slackSend channel: '#ci', message: "Pipeline falhou! Verifique o console. Build #${BUILD_NUMBER}", tokenCredentialId: 'slack-token'
        }
    }
}

