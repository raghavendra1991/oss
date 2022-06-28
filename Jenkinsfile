// Declarative pipeline
pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "raghaduvva/flaskapp"
        DOCKERHUB_CREDENTIALS = credentials('docker-hub')
        CONTAINER_NAME = "app"
        http_proxy = 'http://127.0.0.1:3128/'
        https_proxy = 'http://127.0.0.1:3128/'
        ftp_proxy = 'http://127.0.0.1:3128/'
        socks_proxy = 'socks://127.0.0.1:3128/'
    }

    stages {

        /* We do not need a stage for checkout here since it is done by default when using "Pipeline script from SCM" option. */
        
        stage ('Cleaning') {
            steps {
                echo 'Cleaning Local Images and Containers'
                sh 'docker stop $(docker ps -a -q) || true && docker rm $(docker ps -a -q) || true && docker rmi -f $(docker images  -a -q) || true'     
            }
        }
        stage ('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                sh 'docker build -t $DOCKER_HUB_REPO:$BUILD_NUMBER .'       
            }
        }       
        stage('Create Containers') {
            steps {
                echo 'Creating Conatiner Tesing Purpose'
                sh 'docker run -d --name $CONTAINER_NAME -p 5000:5000 --restart unless-stopped $DOCKER_HUB_REPO:$BUILD_NUMBER && docker ps'
            }
        }
        stage ('Testing Container ') {
            steps {
                echo 'Testing Container'
                sh 'wget localhost:5000'
            }
        }
        stage ('Push Image ') {
            steps {
                echo 'Pushing Image'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR  --password-stdin && docker push $DOCKER_HUB_REPO:$BUILD_NUMBER'
            }
        }
    }
}
