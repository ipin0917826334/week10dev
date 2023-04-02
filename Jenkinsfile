pipeline {
    agent any

    environment {
        DOCKER_CONTAINER_NAME = "lab_docker_jenkins-container"
        DOCKER_IMAGE_NAME     = "lab_docker_jenkins-image"
        PORT                  = "8085"
        DOCKERHUB_COMMON_CREDS = credentials('dockerhub')
    }

    stages {
        stage('Initialize') {
            steps {
                echo 'Initial : Delete  containers and images'
                sh 'docker stop ${DOCKER_CONTAINER_NAME} || true'
                sh 'docker rm ${DOCKER_CONTAINER_NAME} || true'
                sh 'docker rmi ${DOCKER_IMAGE_NAME} || true'
              }
        }


        stage('Build Stage') {
            steps {
                    echo "Current path is ${pwd()}"
                    sh "docker build -t ${DOCKER_IMAGE_NAME} ."
                    sh 'docker login -u $DOCKERHUB_COMMON_CREDS_USR -p $DOCKERHUB_COMMON_CREDS_PSW'
                    sh 'docker lab_docker_jenkins-image ipin0917826334/lab_docker_jenkins-image'
                    sh 'docker image push lab_docker_jenkins-image'
            }
        }

        stage('Run Stage') {
            steps {
                sh "docker run -d -p ${PORT}:3000 --name ${DOCKER_CONTAINER_NAME} ${DOCKER_IMAGE_NAME}"
            }
        }

    
       

    }
}
