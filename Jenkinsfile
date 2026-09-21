pipeline {
    agent { label 'linux' }   // фиксируем агент
    options { timestamps() }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Setup') {
            steps {
                sh 'python3 -m venv venv'
                sh './venv/bin/pip install --upgrade pip'
                sh './venv/bin/pip install -r requirements.txt'
            }
        }
        stage('Build') {
            steps { sh './venv/bin/python -m py_compile app.py' }
        }
        stage('Test') {
            steps { sh './venv/bin/pytest --junitxml=result.xml' }
        }
    }
    post {
        always {
            junit testResults: 'result.xml', allowEmptyResults: true
        }
        success { echo 'Сборка прошла успешно' }
        failure { echo 'Сборка завершилась с ошибкой' }
    }
}
