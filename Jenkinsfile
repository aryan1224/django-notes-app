pipeline{
    agent any
    
    stages{
        stage("Clone Code"){
            steps{
                echo "Cloning the code"
                git url:"https://github.com/aryan1224/django-notes-app", branch:"main"
            }
        }
        stage("Build"){
            steps{
                echo "Building the code"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo "Pushing the image to the Docker Hub"
                withCredentials([usernamePassword(credentialsId:"DockerHubCred", passwordVariable:"DockerHubPass", usernameVariable:"DockerHubUser")]){
                sh "docker tag my-note-app ${env.DockerHubUser}/my-note-app:latest"
                sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPass}"
                sh "docker push ${env.DockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the Container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
