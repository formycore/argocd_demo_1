// original https://github.com/kunchalavikram1427/gitops-demo
pipeline {
    agent any
    environment {
        DOCKERHUB_USERNAME = "formycore"
        APPNAME = "gitops-demo-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" +"${APPNAME}"
        REGISTRY_CREDS = 'dockerhub'
    }
    stages {
        stage ('Clean Workspace') {
            steps {
                script {
                    cleanWs()
                }
            }
        }
        stage ('Checkout SCM') {
            steps {
            git credentialsId: 'github', 
            url: 'https://github.com/formycore/argocd_demo.git',
            branch: 'master'
            }
        }
        stage ('Build docker image'){
            steps {
                script {
                    // using the groovy functions docker.build, so this will take the dockerfile
                    // from the current directory and build it
                    // we already have image_name variable
                    docker_image = docker.build "${IMAGE_NAME} ."
                    // docker build image_name .


                }
            }
        }
        stage ('Push the docker image'){
            steps {
                script {
                    // withRegistry and push or functions  
                    docker.withRegistry ('', REGISTRY_CREDS){
                        docker_image.push("${BUILD_NUMBER}")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage ('Delete docker images locally'){
            steps {
                
                sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                sh "docker rmi ${IMAGE_NAME}:latest"
                
            }
        }
    }
}