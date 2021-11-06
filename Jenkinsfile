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

        stage('Clone Source Code') {
            steps {
                sh '''
                if [ -d big-project ]
                then
                    cd big-project && git pull
                else 
                    git clone https://github.com/prihuda/big-project.git
                fi
                '''
            }
        }

        stage('Docker Build Image') { 
             steps {
                 sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE:${BUILD_NUMBER} ."
             }
         }
         stage('Push Docker Image') { 
             steps {
                 sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:${BUILD_NUMBER}"
             }
         }

           stage('Clone Source Code') {
            steps {
                sh '''
                if [ -d big-project ]
                then
                    ls
                else 
                    git clone https://github.com/prihuda/big-project.git
                fi
                '''
            }
        }

         stage('Deploy Image to Kubernetes') { 
             steps {
                 sh """ sed -i 's;prihuda22/landingpage-sp3:v1 ;prihuda22/sosial-media-bp:${BUILD_NUMBER};g' ./big-project/landing-page/deployment-landing-prod.yaml """
                 sh "kubectl apply -f ./big-project/prod.json"
    	         sh "kubectl apply -f ./big-project/landing-page/deployment-landing-prod.yaml"
                 sh "chmod +x ./big-project/landing-page/prod-landing-service.sh"
                 sh "kubectl delete svc/landing-page -n production"
                 sh "sh ./big-project/landing-page/prod-landing-service.sh"
                 sh "kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.4/deploy/static/provider/aws/deploy.yaml"
                 sh "kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission"
                 sh "kubectl apply -f ./big-project/ingress/ingress.yaml"
             }
         }

         stage('Delete Image') { 
             steps {
                 sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE:${BUILD_NUMBER}"
             }
         }

         stage('Cleaning FIle....') { 
             steps {
                 cleanWs()
             }
         }
    }
}
