pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sharikm/burger-project-web"
        DOCKER_TAG   = "55"
        GIT_REPO     = "https://github.com/sharik-malik/burger-project.git"
        GIT_BRANCH   = "main"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: "${GIT_BRANCH}",
                    credentialsId: 'github-creds',
                    url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                bat """
                docker build -t %DOCKER_IMAGE%:%DOCKER_TAG% .
                """
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat """
                    echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat """
                docker push %DOCKER_IMAGE%:%DOCKER_TAG%
                """
            }
        }
    }

    post {
        always {
            bat "docker logout"
        }
    }
}
