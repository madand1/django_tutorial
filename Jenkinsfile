pipeline {
    agent {
        docker {
            image 'python:3.11' // O la versión que tú usas
        }
    }
    environment {
        PYTHONUNBUFFERED = 1
    }
    stages {
        stage('Instalar dependencias') {
            steps {
                sh '''
                    python -m pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Migraciones (opcional)') {
            steps {
                sh 'python manage.py migrate --noinput'
            }
        }

        stage('Ejecutar tests') {
            steps {
                sh 'python manage.py test'
            }
        }
    }
    post {
        always {
            junit '**/TEST-*.xml' // Si decides usar cobertura/test reporting más adelante
        }
        failure {
            echo '❌ Tests fallidos.'
        }
        success {
            echo '✅ Tests OK.'
        }
    }
}
