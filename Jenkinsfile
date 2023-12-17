pipeline{
    agent any

    environment{

        DOCKERHUB_USERNAME ="desaivishal0206"
        APP_NAME="gitops-argo-app"
        IMAGE_TAG ="${BUILD_NUMBER}"
        IMAGE_NAME ="${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }
    
    stages{
        
        stage('Cleanup workspace'){

            steps{
                script{


                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){

            steps{
                script{
                    // withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {

                    // }
                    git  credentialsId: 'github',
                    url: 'https://github.com/Desaivishal0206/gitops_argocd_project.git',
                    branch: 'main'
                }
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    docker_image = docker.build "${IMAGE_NAME}" 
                }
            }

        }

        stage('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push("latest")
                    }
                }
            }
        }

        stage('Delete Docker Image'){
            steps{
                script{
                    sh"docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh"docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('trigger config change pipeline'){
            steps{
                script{
                    sh"curl -v-k --user vishal:11a51d5e3fa57dd24ab6a7f2a1a77f3c4d -X POST -H 'cache-control:no-cache'-H'content-type: application/x-www-form-urlencoded'--data 'IMAGE_TAG=${IMAGE_TAG}''http://http://3.109.121.88:8080/job/gitops-agrocd_CD/buildWithParameter?token=gitops-config'"
                }
            }
        }

        
    }
}


