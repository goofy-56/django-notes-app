pipeline {
    agent any
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/goofy-56/django-notes-app.git", branch: "dev"
            }
        }
        stage("Build and Test"){
            steps{
                sh "docker build . -t note-app-test-new"
            }
        }
        stage("Push to Docker Hub") {
    steps {
        withCredentials([usernamePassword(
            credentialsId: "dockerHub", 
            passwordVariable: "dockerHubPass", 
            usernameVariable: "dockerHubUser"
        )]) {
            sh "docker tag note-app-test-new ${dockerHubUser}/note-app-test-new:latest"
            sh "echo ${dockerHubPass} | docker login -u ${dockerHubUser} --password-stdin"
            sh "docker push ${dockerHubUser}/note-app-test-new:latest"
        }
    }
}

        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
