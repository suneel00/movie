pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/suneel00/movie.git'
        DOCKER_IMAGE = 'suneel00/movie:${BUILD_NUMBER}'
        DOCKER_CREDENTIALS_ID = 'dockerhub-c'
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                git credentialsId: 'github-c', branch: 'main', url: "${env.GITHUB_REPO}"
            }
        }
        // stage('build and Push image to Docker Hub') {
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
        //             script {
        //                 sh """
        //                     docker build -t ${DOCKER_IMAGE} .
        //                     echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
        //                     docker push ${DOCKER_IMAGE}
        //                 """
        //             }
        //         }
        //     }
        // }
        stage('build and Push image to Docker Hub') {
            steps {
                withDockerRegistry(credentialsId:'DOCKER_PASS') {
                    script {
                        sh """
                            docker build -t ${DOCKER_IMAGE} .
                            docker push ${DOCKER_IMAGE}
                        """
                    }
                }
            }
        }

    }

}
