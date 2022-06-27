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
        
        stage ('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                sh 'docker build -t $DOCKER_HUB_REPO:$BUILD_NUMBER .'       
            }
        }
        stage('Create Containers') {
            steps {
                echo 'Creating Conatiner Tesing Purpose'
                sh 'docker run -d --name $CONTAINER_NAME$BUILD_NUMBER -p $BUILD_NUMBER:5000 --restart unless-stopped $DOCKER_HUB_REPO:$BUILD_NUMBER && docker ps'
            }
        }

        stage ('Push Image ') {
            steps {
                echo 'Pushing Image'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR  --password-stdin && docker push $DOCKER_HUB_REPO:$BUILD_NUMBER'
            }
        }
        stage ('Testing Container ') {
            steps {
                echo 'Testing Container'
                sh 'curl localhost:$BUILD_NUMBER'
            }
        }
        stage ('Deleting Unused Docker Images') {
            steps {
                echo 'Deleting Docker Images'
                sh 'docker rmi -f $(docker images -a -q)'
            }
        }
        
    }
}
