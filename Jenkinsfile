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
                sh "docker build  -t noteapp:latest ."
            }
        }
        
        stage("push to dh") {
            steps {
                echo "Pushing the Docker image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "docker tag noteapp ${DOCKER_USERNAME}/noteapp:latest"
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    sh "docker push ${DOCKER_USERNAME}/noteapp:latest"
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
