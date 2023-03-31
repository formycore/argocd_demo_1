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
                    docker_image = docker.build "${IMAGE_NAME}"
                    // docker build image_name .
                    // we dont need to give the path if we are using that function


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
        stage ('Update the kubernetes deployment file'){
            steps {
                sh "cat deployment.yml"
                sh "sed -i 's/${APPNAME}.*/${APPNAME}:${IMAGE_TAG}/g' deployment.yml"
// the deployment.yml file contains the formycore/gitops-demo-app:11 to get the updated image
// we need to change the deployment.yml file for every build
                sh "cat deployment.yml"
            // to view the deployment.yml file changed or not
            }
        }
        stage ('Push the changed deployment to GIT'){
            steps {
                script {
                    sh """
                        git config --global user.name "sandeep"
                        git config --global user.eamil "formycore@gmail.com"
                        git add deployment.yml
                        git commit -m 'Updated the deployment file'"""
// pushing to git repo we need user name and password
            withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'pass', usernameVariable: 'user')]) {
                sh "http://$user:$pass@github.com/formycore/argocd_demo.git master"
}

                }
            }
        }
    }
}