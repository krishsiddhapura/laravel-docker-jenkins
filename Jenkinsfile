pipeline{
    agent any
    environment {
        IMAGE_NAME = 'krishgrewon/fileupload'
        IMAGE_TAG  = "latest"
    }

    stages {
        stage('Set Commit ID') {
            steps {
                script {
                    env.IMAGE_TAG = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
                    echo "Commit ID set as IMAGE_TAG: ${env.IMAGE_TAG}"
                }
            }
        }

        stage('Build') {
            steps {
                echo "Preparing docker image.."
                script {
                    sh "docker build -f ./docker/Dockerfile -t $IMAGE_NAME:$IMAGE_TAG -t $IMAGE_NAME:latest ."
                }
            }
        }

        stage('Push') {
            steps {
                echo "Pushing docker image.."
                script {
                    withCredentials([usernamePassword(credentialsId: 'dc631ca1-369e-4ef5-b7b2-240bf7710d60', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    }
                    sh "docker push $IMAGE_NAME:$IMAGE_TAG"
                    sh "docker push $IMAGE_NAME:latest"
                }
            }
        }

        stage('Production Release'){
            steps {
                script {
                    sh "docker compose -f /Users/kri55h/Sites/localhost/demo/docker-compose/docker-compose.yaml up"
                }
            }
        }
    }
}
