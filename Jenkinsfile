pipeline {
    agent any
    stages {
        stage('Code') {
            steps {
                git url: 'https://github.com/nowsmart/flask-app.git', branch: 'main'
                echo 'Code cloned....'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t flask-app .'
                echo 'Code build done...'
            }
        }
        stage('Test'){
            steps {
                echo 'Test done'
            }
        }
        stage('Push to Docker Hubb'){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCreds',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass')]){
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                        sh "docker image tag flask-app ${env.dockerHubUser}/flask-app:latest"
                        sh "docker push ${env.dockerHubUser}/flask-app:latest"
                    }
            }
        }
        stage('Development') {
            steps {
                sh "docker-compose down"
                sh "docker-compose up --build -d"
            }
        }
    }
}
