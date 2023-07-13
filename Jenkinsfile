pipeline {
    agent any
    stages {
        stage('Code') {
            steps {
               echo "Cloning the code."
               git url: "https://github.com/ajinkyak423/django-notes-app.git", branch: "main"
            }
        }
        
        stage("Build"){
            steps {
               echo "Building the code."
               sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
               echo "Pushing to the Docker Hub."
               withCredentials([usernamePassword(credentialsId:"dockerHub", passwordVariable:"dockerHubPass", usernameVariable: "dockerHubUser")]){
               sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:v1"
               sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
               sh "docker push ${env.dockerHubUser}/my-note-app:v1"
               }
            }
        }
        stage("Deploy"){
            steps {
               echo "Deploying the container"
               sh "docker-compose down && docker-compose up -d"
            }
        }
        
    }
}
