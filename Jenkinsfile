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
                sh 'docker buildx build -t jjulianbayon/buildx:latest -f Dockerfile .'
             }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Publish') {
            steps {
                dir ('Dockerfiles'){
                echo 'Publishing image to DockerHub..'
                sh 'docker buildx build --push --platform linux/amd64,linux/arm64 -t jjulianbayon/buildx:latest -f Dockerfile .'
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

