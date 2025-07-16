pipeline{
    agent any
    environment {
        IMAGE_NAME = 'krishgrewon/fileupload'
        IMAGE_TAG  = "latest"
    }

    stages {
        stage('Build') {
            steps {
                echo "Preparing docker image.."
                script {
                    sh "docker build -f ./docker/Dockerfile -t $IMAGE_NAME:$IMAGE_TAG -t $IMAGE_NAME:latest ."
                }
            }
        }
    }
}
