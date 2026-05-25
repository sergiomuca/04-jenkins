pipeline {
    agent any // Se ejecuta en el nodo principal de Jenkins [10]

    options {
        disableConcurrentBuilds() // Deshabilita builds concurrentes [8, 11]
        timestamps() // Muestra marcas de tiempo
        timeout(time: 5, unit: 'MINUTES') // Timeout de 5 minutos
    }

    environment {
        FORCE_COLOR = '0' // Variable de entorno numérica [7]
        NO_COLOR = 'true' // Variable de entorno booleana [7]
    }

    stages {
        stage('Audit tools') {
            steps {
                sh 'node --version' // Imprime la versión de node [12]
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm install' // Instala dependencias [13]
            }
        }

        stage('Generate files') {
            steps {
                sh 'npm run prisma:generate' // Ejecuta prisma generate
            }
        }

        stage('Format check') {
            steps {
                sh 'npm run format:check' // Verifica el formato [14]
            }
        }

        stage('Code quality') {
            steps {
                sh 'npm run lint' // Verifica la calidad del código [15]
            }
        }

        stage('Type check') {
            steps {
                sh 'npm run type-check' // Comprobación de tipos [15]
            }
        }

        stage('Tests') {
            steps {
                sh 'npm run test' // Ejecución de tests [15]
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build' // Construcción de la solución [16]
                // Archiva los artefactos del directorio dist/ con fingerprint activo [17]
                archiveArtifacts artifacts: 'dist/**', fingerprint: true 
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!' // Mensaje de éxito [18]
        }
        failure {
            echo 'Pipeline failed. Review logs.' // Mensaje de error [18]
        }
        always {
            cleanWs() // Limpia el workspace siempre al finalizar [19]
        }
    }
}