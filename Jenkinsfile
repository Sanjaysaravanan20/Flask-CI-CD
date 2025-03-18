pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sanjay2025/flask-app"
        DOCKER_CREDENTIALS = "docker-hub-credentials"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Sanjaysaravanan20/Flask-CI-CD.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:latest ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: DOCKER_CREDENTIALS, url: 'https://index.docker.io/v1/']) {
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh "ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i hosts.ini playbook.yml"
            }
        }
    }
}
