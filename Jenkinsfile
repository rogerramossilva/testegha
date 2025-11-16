pipeline {
    agent any
<<<<<<< HEAD
    environment {
        DOCKERFILE_WEB = 'Dockerfileweb'       // Nome do Dockerfile para a imagem web
        DOCKERFILE_DB = 'Dockerfiledb'         // Nome do Dockerfile para a imagem db
        DOCKERFILE_NGINX = 'Dockerfilenginx'   // Nome do Dockerfile para a imagem nginx
        DOCKER_IMAGE_TAG = 'latest'            // Tag da imagem Docker a ser construída
    }
    stages {

        stage('Build Images') {
            steps {
                script {
                    // Nome das imagens
                    def imageNameWeb = 'rogerramossilva/web'
                    def imageNameDB = 'rogerramossilva/db'
                    def imageNameNginx = 'rogerramossilva/nginx'

                    // Executa o build das imagens com os respectivos Dockerfiles
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhubrogerio') {
                        def webImage = docker.build("${imageNameWeb}:${DOCKER_IMAGE_TAG}", "-f ${DOCKERFILE_WEB} .")
                        def dbImage = docker.build("${imageNameDB}:${DOCKER_IMAGE_TAG}", "-f ${DOCKERFILE_DB} .")
                        def nginxImage = docker.build("${imageNameNginx}:${DOCKER_IMAGE_TAG}", "-f ${DOCKERFILE_NGINX} .")

                        webImage.push()
                        dbImage.push()
                        nginxImage.push()
=======

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
>>>>>>> develop
                    }
                }
            }
        }
<<<<<<< HEAD
        stage('Teste Aplicação') {
            steps {
                // Testes automatizados da aplicação
		sh 'docker rm -f $(docker ps -a -q)'
                sh 'docker-compose -f docker-compose.yml up -d'

                // Realize os testes nos serviços em execução
                // Exemplo de teste usando curl para verificar se o serviço web está respondendo
                sh 'curl -I http://localhost'

                sh 'docker-compose -f docker-compose.yml down'
            }
        }
        stage('Aprovação') {
            steps {
                input 'Deseja prosseguir com o Deploy e Push?'
            }
        }

 }
}
=======

        stage('Security Scan (Trivy)') {
            steps {
                slackSend channel: '#ci', message: "Rodando Trivy Scan...", tokenCredentialId: 'slack-token'

                sh """
                    trivy image --severity HIGH,CRITICAL --exit-code 1 ${IMAGE_WEB}
                    trivy image --severity HIGH,CRITICAL --exit-code 1 ${IMAGE_DB}
                    trivy image --severity HIGH,CRITICAL --exit-code 1 ${IMAGE_NGINX}
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

>>>>>>> develop
