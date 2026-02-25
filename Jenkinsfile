// Jenkinsfile — Declarative Pipeline для Go-проекта
pipeline {
    agent any
    
    environment {
        // Переменные для повторного использования
        DOCKER_IMAGE = "my-go-app"
        GO_ENTRYPOINT = "/home/vboxuser/sdvps-materials-8-03/main.go"  // ← ИЗМЕНИТЕ на путь к вашему main.go
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Клонируем репозиторий (автоматически через SCM)
                checkout scm
                echo "📦 Исходный код получен"
            }
        }
        
        stage('Test') {
            steps {
                echo "🔍 Запуск тестов Go..."
                sh '''
                    go version
                    go test -v -cover ./...
                '''
            }
        }
        
        stage('Build Binary') {
            steps {
                echo "🔨 Сборка Go-бинарника..."
                sh '''
                    # Сборка статического бинарника для Linux
                    CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app ${GO_ENTRYPOINT}
                    
                    # Проверка, что файл создан
                    ls -lh app
                '''
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo "🐳 Сборка Docker-образа..."
                script {
                    // Используем docker CLI напрямую (проще и надёжнее)
                    sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ."
                    sh "docker images | grep ${DOCKER_IMAGE}"
                }
            }
        }
    }
    
    post {
        // Действия после завершения pipeline
        always {
            // Очищаем workspace, чтобы не занимать место
            cleanWs()
        }
        success {
            echo "✅ Сборка завершена успешно! Образ: ${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
        }
        failure {
            echo "❌ Ошибка сборки! Проверьте Console Output"
            // Можно добавить отправку уведомления здесь
        }
    }
}
