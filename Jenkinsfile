pipeline {
    agent {label "dev"};

    environment {
        DOCKER_HUB_REPO = "shafaullahagha/flask-app"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage("Code Clone") {
            steps {
                git url: "https://github.com/shafaullahagha/two-tier-flask-app.git", branch: "master"
            }
        }

        stage("Build Image") {
            steps {
              sh "docker build -t ${DOCKER_HUB_REPO}:${TAG} ."
            }
        }

        stage("Test") {
            steps {
                echo "Developer / Tester tests likh ke dega"
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    usernameVariable: "DOCKER_USER",
                    passwordVariable: "DOCKER_PASS"
                )]) {

                    sh """
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push ${DOCKER_HUB_REPO}:${TAG}
                    """
                }
            }
        }

        stage("Deploy") {
            steps {
                sh """
                docker compose pull
                docker compose up -d
                """
            }
        }
    }
post {
    success {
        emailtext(
            subject: "Build Successful",
            body: "Good News: Your build was successful!",
            to: "shafaullahagha@gmail.com"
        )
    }
    failure {
         emailtext(
            subject: "Build Failed",
            body: "Bad News: Your build Failed!",
            to: "shafaullahagha@gmail.com"
        )
    }
}    
}
