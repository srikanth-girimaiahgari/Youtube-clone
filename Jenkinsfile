pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node'
    }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/srikanth-girimaiahgari/Youtube-clone.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
                       sh "docker build -t youtube-clone ."
                       sh "docker tag youtube-clone sr79979/youtube-clone:latest "
                       sh "docker push sr79979/youtube-clone:latest "
                       sh "docker "
                    }
                }
            }
        }
        stage('Deploy to container'){
            steps{
                sh 'docker run -d --name youtube-clone -p 3000:3000 sr79979/youtube-clone:latest'
            }
        }
    }
     post {
        success {
            slackSend(channel: '#all-the-cloud-hub', color: 'good', message: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}) was successful.")
        }
        failure {
            slackSend(channel: '#all-the-cloud-hub', color: 'danger', message: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}) failed.")
        }
        always {
            echo 'Build finished, check Slack for notifications.'
        }
    }
}
