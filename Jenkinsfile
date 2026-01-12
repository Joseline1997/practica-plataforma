pipeline {
    agent any

    stages {
        stage('Instalar y Testear (En Contenedor)') {
            agent {
                docker { 
                    // Usamos una imagen que YA tiene Node instalado. 
                    // Así Jenkins no tiene que descargar nada.
                    image 'node:20-alpine' 
                    // Esto asegura que use los archivos de tu proyecto
                    reuseNode true 
                }
            }
            steps {
                sh 'node --version' // Para verificar que funciona
                sh 'npm install'
                sh 'chmod +x ./node_modules/.bin/jest'
                sh 'npm test -- --ci --runInBand'
            }
        }

        stage('Construir y Desplegar (En Host)') {
            // Esta etapa corre fuera del contenedor de Node, 
            // usando el Docker de tu sistema principal.
            steps {
                sh 'docker build -t hola-mundo-node:latest .'
                sh '''
                    docker stop hola-mundo-node || true
                    docker rm hola-mundo-node || true
                    docker run -d --name hola-mundo-node -p 3000:3000 hola-mundo-node:latest
                '''
            }
        }
    }
}