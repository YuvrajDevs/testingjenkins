pipeline {
    agent any

    tools {
        nodejs 'NodeJS-LTS'
    }

    // Define variables for our pipeline
    environment 
        DOCKERHUB_USERNAME = 'yuvrajdevs'
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/jenkins-project"
        IMAGE_TAG = "build-${BUILD_NUMBER}" // BUILD_NUMBER is a special Jenkins variable
    }

    stages {
        stage('Build & Test App') {
            steps {
                echo '--- INSTALLING DEPENDENCIES & RUNNING TESTS ---'
                sh 'npm install'
                sh 'npm test'
            }
        }

        // NEW STAGE to build the Docker image
        stage('Build Docker Image') {
            steps {
                echo "--- BUILDING DOCKER IMAGE: ${IMAGE_NAME}:${IMAGE_TAG} ---"
                // This command uses the Dockerfile in our repository
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        // NEW STAGE to push the Docker image
        stage('Push Docker Image') {
            steps {
                echo "--- PUSHING DOCKER IMAGE TO DOCKER HUB ---"
                // This 'withCredentials' block securely loads our saved password
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    // We use the loaded credentials to log in to Docker Hub
                    sh "docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}"
                    // Then we push the image
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }
}
