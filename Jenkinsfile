@Library("Github_library") _
pipeline {
    agent { label "notes-app" }

    stages {
        stage("Hello") {
            steps {
                hello()
            }
        }

        stage("Code") {
            steps {
                gitclone("https://github.com/devops-pratham/django-notes-app.git", 'main')
            }
        }

        stage("Build") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHubCredentials',
                                                 usernameVariable: 'DOCKER_HUB_USER',
                                                 passwordVariable: 'DOCKER_HUB_PASS')]) {
                    docker_build(
                        imageName: "${DOCKER_HUB_USER}/jenkins",
                        imageTag: "notesapp",
                        dockerfile: "Dockerfile",
                        context: "."
                    )
                }
            }
        }

        stage("Push") {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'DockerHubCredentials',
                                             usernameVariable: 'DOCKER_HUB_USER',
                                             passwordVariable: 'DOCKER_HUB_PASS')]){
                    dockerPush(
                        imageName: "${env.DOCKER_HUB_USER}/jenkins",
                        imageTag: "notesapp",
                        credentials: "DockerHubCredentials"
                    )
                }    
              }
            }
        }

        stage("Deploy") {
            steps {
                script{
                    deploy("notesapp.yml")
                }
            }
        }
    }
}
