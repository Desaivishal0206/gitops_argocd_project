pipeline{
    agent any

    environment{

        DOCKERHUB_USERNAME ="desaivishal0206"
        APP_NAME="gitops-argo-app"
        IMAGE_TAG ="${BUILD_NUMBER}"
        // IMAGE_NAME ="${DOCKERHUB_USERNAME}" 
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
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {

                    }
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

        // stage('Push Docker Image'){
        //     steps{
        //         script{
        //             docker.withRegister('',REGISTRY_CREDS){
        //                 docker_image.push("$BUILD_NUMBER")
        //                 docker_image.push("latest")
        //             }
        //         }
        //     }
        // }
    }
}

// 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
