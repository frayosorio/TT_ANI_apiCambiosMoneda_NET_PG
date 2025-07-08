pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'apicambiosmonedatt'
        CONTAINER_NAME = 'dockerapicambiosmonedatt'
        APP_PORT = '5235'
        HOST_PORT = '7080' // Cambia si quieres mapear a otro puerto en la máquina host
        DOCKER_NETWORK = 'dockermonedas_red' // Red compartida con POSTGRES
    }

    
    stages {

        stage('Ejecutar pruebas unitarias') {
            steps {
                bat "dotnet test apiCambiosMoneda.Test\\apiCambiosMoneda.Test.csproj --configuration Release"
            }
        }
        stage('Construir imagen Docker') {
            steps {
                bat "docker build . -t ${DOCKER_IMAGE}"
            }
        }
        stage('Limpiar contenedor existente') {
            steps {
                bat "docker rm -f ${CONTAINER_NAME} || exit 0"
            }
        }
        stage('Ejecutar contenedor'){
            steps{
                bat "docker run --network ${DOCKER_NETWORK} --name ${CONTAINER_NAME} -p ${HOST_PORT}:${APP_PORT} -d ${DOCKER_IMAGE}"
            }
        }
    }
}