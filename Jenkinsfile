pipeline {
    agent any 
    
    stages {
        stage("code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/adguchiya/jenkins-cicd.git", branch: "main"
            }
        }
        
        stage("build") {
            steps {
                echo "Building the Docker image"
                sh "docker build  -t todoapp:latest ."
            }
        }
        
        stage("push to dh") {
            steps {
                echo "Pushing the Docker image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "docker tag todoapp ${DOCKER_USERNAME}/todoapp:latest"
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    sh "docker push ${DOCKER_USERNAME}/todoapp:latest"
                }
                
            }
        }
        
        stage("deploy") {
            steps {
                echo "Deploying the Docker container"
                sh "docker-compose down"
                sh "docker-compose up -d --force-recreate --no-deps --build web"
            }
        }
    }
}
