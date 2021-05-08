pipeline {
    agent any
    stages {
        stage("build") {
            steps {
                script {
                    echo "Building solution..."
                    sh "dotnet build -c:Release jenkins-demo.sln"
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    echo "Building docker image..."
                    withCredentials([usernamePassword(credentialsId: "nexus-docker-repo", usernameVariable: 'USER', passwordVariable: 'PWD')]) {
                        sh "docker build -t host.docker.internal:4002/jenkins-demo:1.2 -f web-api/Dockerfile ./web-api"
                        sh "docker login -u ${USER} -p ${PWD} host.docker.internal:4002"
                        sh "docker push host.docker.internal:4002/jenkins-demo:1.2"
                    }                    
                }
            }
        }
    }
}