pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {
                echo 'checkout source code from github'
                git url:"https://github.com/Naveensrn1/django-notes-app.git", branch: "main"
            }
        }
        stage('build') {
            steps {
                echo 'build the java application'
                sh "docker build -t my-notes-app ."
            }
        }
        stage('push to DockerHub') {
            steps {
                echo 'push the image to docker hub'
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker tag my-notes-app ${env.dockerHubUser}/my-notes-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/my-notes-app:latest"
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploying the contaniner'
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
