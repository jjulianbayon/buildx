pipeline {
    agent any

    stages {
        stage('Init') {
            steps {
                echo 'Initializing..'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                echo "Current branch: ${env.BRANCH_NAME}"
            }
        }
        stage('Build') {
            steps {
                echo 'Building image..'
                def buildImage = docker.build("test-image", "./") 
                buildImage.inside {
                    sh 'docker buildx build -t  buildx:latest -f Dockerfile .'
                }
            }
        }   
        stage('Publish') {
            steps {
                dir ('Dockerfiles'){
                echo 'Publishing image to DockerHub..'
                def testImage = docker.build("test-image", "./") 
                testImage.inside {
                sh 'docker buildx create --name multiarch --driver docker-container --use -t jjulianbayon/buildx:latest -f Dockerfile .'
                 }
             }    
           }
        }
        stage('Cleanup') {
            steps {
                echo 'Removing unused docker containers and images..'
                sh 'docker ps -aq | xargs --no-run-if-empty docker rm -f'
                // keep intermediate images as cache, only delete the final image
                sh 'docker images -q | xargs --no-run-if-empty docker rmi'
            }
        }
    }
}
