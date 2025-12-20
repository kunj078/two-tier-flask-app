pipeline{
    agent any;
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/kunj078/two-tier-flask-app.git", branch: "master"
                echo "Code phase Done"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t flask-app ."
                echo "Build phase"
            }
        }
        stage("Test"){
            steps{
                echo "Test phase"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo "Push On Docker Hub Start"
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag flask-app ${env.dockerHubUser}/flask-app"
                    sh "docker push ${env.dockerHubUser}/flask-app:latest"
                }
                echo "Push On Docker Hub End"
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
                echo "Deploy phase"
            }
        }
    }
}