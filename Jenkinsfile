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

        stage('Updating kubernates deployment file'){
            steps{
                script{
                    sh"""
                    cat deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                    cat deployment.yml
                    """
                }
            }

        }
    }
}

// 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
