pipeline {
    agent any

    stages {
        stage('Build & Push Docker Image') {
            steps {
                withCredentials([
                    string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                    export AWS_DEFAULT_REGION="ap-south-1"

                    AWS_ACCOUNT_ID="204625684000"
                    IMAGE_REPO_NAME="mywebsite"
                    IMAGE_TAG="latest"
                    PROJECT_DIR="/project"

                    aws ecr describe-repositories --repository-names $IMAGE_REPO_NAME --region $AWS_DEFAULT_REGION || \
                    aws ecr create-repository --repository-name $IMAGE_REPO_NAME --region $AWS_DEFAULT_REGION

                    aws ecr get-login-password --region $AWS_DEFAULT_REGION | \
                    docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com

                    cd $PROJECT_DIR
                    docker build -t $IMAGE_REPO_NAME:$IMAGE_TAG .
                    docker tag $IMAGE_REPO_NAME:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG
                    docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG
                    '''
                }
            }
        }
    }
}

