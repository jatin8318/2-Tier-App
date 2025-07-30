@Library("Shared") _
pipeline{
    
    agent { label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
               script{
                   clone("https://github.com/LondheShubham153/two-tier-flask-app.git", "master")
               }
            }
        }
        stage("Trivy File System Scan"){
            steps{
                script{
                    trivy_fs()
                }
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
            
        }
        stage("Push To DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockeridpass",
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]){
                sh 'docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}'
                sh "docker image tag 2-tier-app:latest ${env.dockerHubUser}/2-tier-app:latest"
                sh "docker push ${env.dockerHubUser}/2-tier-app:latest"
                }
            }
        }
