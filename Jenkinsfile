@Library("Shared") _
pipeline {
    agent {label "dev"};

    environment {
        DOCKER_HUB_REPO = "shafaullahagha/flask-app"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage("Code Clone") {
            steps {
                script{
                clone("https://github.com/shafaullahagha/two-tier-flask-app.git", "master")
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
                script{
                    docker_push("dockerHubCreds","DOCKER_HUB_REPO","TAG" )
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
    
post{
        success{
            script{
                emailext from: "shafaullahagha@gmail.com",
                to: "shafaullahagha@gmail.com",
                body: "Build Successfuly",
                subject: "Jenkins Build successful"   
             }        
        }
        failure{
            script{
                emailext attachLog: true,
                to: "shafaullahagha@gmail.com",
                body: "Build Failed",
                subject: "Jenkins Build Failed"
            }
        }
    }    
}
