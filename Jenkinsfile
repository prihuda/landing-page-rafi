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
        // stage('Docker Build Image') { 
        //      steps {
        //          sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE:${BUILD_NUMBER} ."
        //      }
        //  }
         stage('Push Docker Image') { 
             steps {
                 sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:${BUILD_NUMBER}"
             }
         }
         stage('Deploy Image to Kubernetes') { 
             steps {
                 sh "git clone https://github.com/prihuda/big-project.git"
                 sh """ sed -i 's;prihuda22/sosial-media-bp ;prihuda22/sosial-media-bp:${BUILD_NUMBER};g' ./big-project/landing-page/deployment-landing-prod.yaml """
    	         sh "kubectl apply -f ./big-project/landing-page/deployment-landing-prod.yaml"
                 sh "chmod +x ./big-project/landing-page/prod-landing-service.sh"
                 sh "sh ./big-project/landing-page/prod-landing-service.sh"
                 sh "kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.4/deploy/static/provider/aws/deploy.yaml"
                 sh "kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission"
                 sh "kubectl apply -f ./big-project/ingress/ingress.yaml"
             }
         }
    }
}
