
pipeline 
{

    agent any
    environment {
        // Docker Hub credentials ID stored in Jenkins
         DOCKERHUB_CREDENTIALS = 'docker-id'
         IMAGE_NAME = 'amalan06/gameappnewimage'
    }

    stages 
    {
        stage('Cloning Git'){
            steps{
                checkout scm
            }
        }

        stage('SAST')
        {
            steps
            {
                sh 'echo Running SAST scan...'
            }
        }

        stage('BUILD-AND-TAG')
        {
            {
                agent {label 'CWEB-2140-60-Appserver-Amalan' }
            }

            steps
            {
                script 
                {
                     // Build Docker image using Jenkins Docker Pipeline API 
                     echo "Building Docker image ${IMAGE_NAME}..."
                     app = docker.build("${IMAGE_NAME}")
                     app.tag("latest")
                }

            }
        }

    //     stage('POST-TO-DOCKERHUB')
    //     {
    //         {
    //             agent {label 'CWEB-2140-60-Appserver-Amalan' }
    //         }

    //         steps
    //         {
    //             script
    //             {
    //                 echo "Pushing image ${IMAGE_NAME}:latest to Docker Hub.."
    //                 docker.withRegistry('https://registry.hub.docker.com', "${DOCKERHUB_CREDENTIALS}")
    //             }
    //         }

    //     }

    //     stage('DEPLOYMENT')
    //     {
    //         {
    //             agent {label 'CWEB-2140-60-Appserver-Amalan' }
    //         }

    //         steps
    //         {
    //             echo 'Starting deployment using docker-compose...'
    //             script
    //             {
    //                dir("${WORKSPACE}") 
    //                {
    //                 sh '''
    //                     docker-compose down
    //                     docker-compose up -d
    //                     docker ps
    //                 '''
    //                }
    //             }
    //             echo 'Deployment completed successfully!'
    //         }
    //     }


    }


}