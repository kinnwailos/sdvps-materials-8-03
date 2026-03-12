pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-go-app"
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
                sh 'go test -v -cover ./...'
            }
        }

        stage('Build Binary') {
            steps {
                sh '''
                    CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app
                    ls -lh app
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ."
            }
        }

        // 🔥 ИСПРАВЛЕННЫЙ STAGE ДЛЯ NEXUS
        stage('Upload to Nexus') {
            steps {
                sh '''
                    echo "📤 Загрузка в Nexus..."

                    # Проверка файла
                    if [ ! -f app ]; then
                        echo "❌ Файл app не найден!"
                        exit 1
                    fi

                    # Загрузка (ЗАМЕНИТЕ ПАРОЛЬ НА ВАШ!)
                    curl -v -u "admin:123000///123" \
                         --upload-file app \
                         http://localhost:8081/repository/go-binaries/app-${BUILD_NUMBER}

                    echo "✅ Загрузка завершена"
                '''
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
    }
}
