
pipeline {
    agent any
    
    stages {
        stage("Clonammos el repositorio") {
            steps {
                git clone 'https://github.com/edgaregonzalez/nodejs-helloworld-api.git'
            }
        }
        stage("Instalar Dependencias") {
            steps {
                sh 'npm install'
            }
        }
        stage("Ejecutar Pruebas") {
            steps {
                sh 'npm test'
            }
        }
        stage('Iniciar Aplicación') {
            steps {
                sh 'npm start &'
            }
        }
         stage('Probar Aplicación') {
            steps {
                // Esperar unos segundos para que la aplicación se inicie completamente
                sleep time: 30, unit: 'SECONDS'
                sh 'curl http://localhost:3000'
            }
        }
    }
}
