env.DOCKER_REGISTRY = 'prihuda22'
env.DOCKER_IMAGE = 'sosial-media-bp'


pipeline {
    agent any

    stages {
        stage('Hello World') { 
            steps {
                sh "whoami"
            }
        }
        stage('Docker Build Image') { 
             steps {
                 sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE:${BUILD_NUMBER} ."
             }
         }
    }
}
