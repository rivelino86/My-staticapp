pipeline {
    agent any

    parameters {
        string(name: 'VERSION', defaultValue: '1.2.0', description: 'App to deploy')
    }

    environment {
        CRED_ID = 'ecr:us-east-1:clinic_app'
        REPO_NAME = 'my-clinicapp'
        REPO_URL = "655040006853.dkr.ecr.us-east-1.amazonaws.com"
        FULL_REPO_URL = "https://${REPO_URL}"
        CLUSTER_NAME = 'clinic-cluster'
        SERVICE_NAME = 'clinic-app-service'
        AWS_REGION = 'us-east-1'
    }

    stages {
        
        stage("Build & Push to ECR") {
            steps {
                script {
                    withDockerRegistry(credentialsId: CRED_ID, url: FULL_REPO_URL) {
                        echo "***********I am a DevOps Engineer**********"
                        
                        sh "docker build -t clinicapp:${params.VERSION} ."
                        
                        sh "docker tag clinicapp:${params.VERSION} ${REPO_URL}/${REPO_NAME}:${params.VERSION}"
                        sh "docker tag clinicapp:${params.VERSION} ${REPO_URL}/${REPO_NAME}:latest"
                        
                        sh "docker push ${REPO_URL}/${REPO_NAME}:latest"
                        sh "docker push ${REPO_URL}/${REPO_NAME}:${params.VERSION}"
                    }
                }
            }
        stage( "install AWSCLI"){
            steps{
              sh '''

                   $ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                   unzip awscliv2.zip
                   sudo ./aws/install

              '''
            }
        }
          
        }

        stage("Update ECS") {
            steps {
                sh "aws ecs update-service --cluster ${CLUSTER_NAME} --service  ${SERVICE_NAME} --region ${AWS_REGION}  --force-new-deployment"
            }
        }
    }
}
