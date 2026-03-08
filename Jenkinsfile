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
                echo "🔍 Запуск тестов Go..."
                sh 'go test -v -cover ./...'
            }
        }
        
        stage('Build Binary') {
            steps {
                echo "🔨 Сборка Go-бинарника..."
                sh '''
                    CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app
                    ls -lh app
                '''
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo "🐳 Сборка Docker-образа..."
                sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ."
            }
        }
        
        stage('Upload to Nexus') {
            steps {
                sh '''
                    echo "📤 Загрузка в Nexus..."
                    curl -v -u admin:Nexus123! \
                         --upload-file app \
                         http://localhost:8081/repository/go-binaries/app-${BUILD_NUMBER}
                    echo "✅ Загружено!"
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
        failure {
            echo "❌ Ошибка сборки!"
        }
    }
}
