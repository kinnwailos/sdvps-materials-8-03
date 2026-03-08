// Jenkinsfile — исправленная версия
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-go-app"
        // GO_ENTRYPOINT = "./main.go"  // Раскомментируйте, если main.go не в корне
    }

    stages {
        stage('Checkout') {
            steps {
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
                    # Если main.go в корне - просто:
                    CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app

                    # Если main.go в поддиректории - укажите относительный путь:
                    # CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app ./cmd/main.go

                    ls -lh app
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Сборка Docker-образа..."
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ."
                }
            }
        }
    }


    post {
        always {
            cleanWs()
        }
        success {
            echo "✅ Сборка завершена успешно!"
        }
        failure {
            echo "❌ Ошибка сборки!"
        }
    }
}
stage('Upload to Nexus') {
    steps {
        sh '''
            curl -v -u admin:Nexus123! \
                 --upload-file app \
                 http://localhost:8081/repository/go-binaries/app-${BUILD_NUMBER}
        '''
    }
}