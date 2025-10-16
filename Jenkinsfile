
pipeline 
{

    agent any
    environment {
        // Docker Hub credentials ID stored in Jenkins
         DOCKERHUB_CREDENTIALS = 'docker-id'
         IMAGE_NAME = 'penbeaa/gameappnewimage'
    }

    stages 
    {
        stage('Cloning Git'){
            steps{
                checkout scm
            }
        }

        stage('SAST-TEST')
        {
            agent any
            steps
            {
                script
                {
                    synkSecurity(
                        synkInstallation: 'Snyk-installations',
                        synkTokenId: '0d81bea7-bb0c-4755-817c-d9229518398c',
                        severity: 'critical'
                        
                    )
                }
            }
        }

        stage('BUILD-AND-TAG')
        {
            
             agent {label 'CWEB-2140-60' }
           

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

        stage('POST-TO-DOCKERHUB')
        {
            
                agent {label 'CWEB-2140-60' }
            

            steps
            {
                script
                {
                    echo "Pushing image ${IMAGE_NAME}:latest to Docker Hub.."
                    docker.withRegistry('https://registry.hub.docker.com', "${DOCKERHUB_CREDENTIALS}")
                    {
                        app.push("latest")
                    } 

                }
            }

        }

        stage('DEPLOYMENT')
        {
            
                agent {label 'CWEB-2140-60' }
            

            steps
            {
                echo 'Starting deployment using docker-compose...'
                script
                {
                   dir("${WORKSPACE}") 
                   {
                    sh '''
                        docker-compose down
                        docker-compose up -d
                        docker ps
                    '''
                   }
                }
                echo 'Deployment completed successfully!'
            }
        }


    }


}
