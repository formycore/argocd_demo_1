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
    }
}