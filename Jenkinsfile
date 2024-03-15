pipeline {
  agent any
  environment {
        IMAGE_REPO_NAME="frontend_angular"
        IMAGE_TAG="v1"
        AWS_DEFAULT_REGION="eu-west-3"
        AWS_ACCOUNT_ID="992382586240"
        REPOSITORY_URI="992382586240.dkr.ecr.eu-west-3.amazonaws.com/hackathon"
  }
  stages {
    stage("BUILD DOCKER IMAGE") {
      steps {
        script {
          dockerImageBuild = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        }
      }
    }
     stage("DEPLOY DOCKER") {
       steps {
          sh """aws ecr get-login-password | docker login --username AWS --password-stdin ${REPOSITORY_URI}"""
          sh """docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"""
          sh """docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"""
      }
   }
    stage("DEPLOY & ACTIVATE") {
      steps {
        sh """aws ecs run-task --cluster test-cluster --count 1 --launch-type FARGATE --task-definition new-task-2 --network-configuration "awsvpcConfiguration={subnets=[subnet-047a9ba7de6881bc3
],securityGroups=[sg-0d1d32fc90ea0ebcc],assignPublicIp=ENABLED}"
"""
      }
    }
  }
}
