pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                echo 'Clonando o repositório do jogo...'
                git 'https://github.com/luishenriquemorais/guess_game.git'
            }
        }
        stage('Build and Deploy') {
            steps {
                script {
                    dir('guess_game') {
                        echo 'Iniciando build da imagem Docker e deploy do contêiner...'
                        sh 'docker-compose up --build -d'
                        echo 'Aplicação "guess_game" iniciada com sucesso!'
                        echo 'Acesse em http://<IP_DO_SEU_DOCKER_HOST>:5000'
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline finalizado.'
        }
    }
}
