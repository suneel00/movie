pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/suneel00/movie.git'
        DOCKER_IMAGE = 'suneel0/movie:${BUILD_NUMBER}'
        REGISTRY_CREDENTIALS = credentials('dockerhub-c')
        
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                git credentialsId: 'github-c', branch: 'main', url: "${env.GITHUB_REPO}"
            }
        }

        stage('Build Docker Image') {

            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                    def dockerImage=docker.image("${DOCKER_IMAGE}")
                    docker.withRegistry("https://index.docker.io/v1/","dockerhub-c"){
                        dockerImage.push()
                    }
                }
            }
        }

        // stage('Push to Docker Hub') {
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
        //             script {
        //                 sh """
        //                     echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
        //                     docker push ${DOCKER_IMAGE}:${IMAGE_TAG}
        //                 """
        //             }
        //         }
        //     }
        // }

    }

}