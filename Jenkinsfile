pipeline{
    agent any

     parameters {
        string (name: 'VERSION', defaultValue: '1.0.0')
     }
      environment{
        CRED_ID = 'ecr:us-east-1:clinic_app'
        REPO_NAME = 'My-staticapp'
        REPO_URL = "655040006853.dkr.ecr.us-east-1.amazonaws.com"
        FULL_REPO_URL = 'http://${URL}/'
        CLUSTER_NAME = 'clinic-cluster'
        SERVICE_NAME = 'clinic-app-service'
       
      }
   
    stages{
        
        stage("Build & push to ECR"){

        
        steps{
            script{
                // This step should not normally be used in your script. Consult the inline help for details.
                 withDockerRegistry(credentialsId: CRED_ID , url: FULL_REPO_URL ){
                        echo ' "***********I am a DevOps Engineer*********" '

                        sh "docker build -t clinicapp:${params.VERSION} ."

                        sh "docker tag clinicapp:${params.VERSION} ${REPO_UR}/${REPO_NAME}:${params.VERSION}"

                        sh "docker tag clinicapp:${params.VERSION} ${REPO_URL}/${REPO_NAME}:latest"

                        sh "docker push ${REPO_URL}/${REPO_NAME}:latest"

                        sh "docker push ${REPO_URL}/${REPO_NAME}:${params.VERSION}"
                 }
               }
        }
            }
    }
            stage("uptade ECS"){
                steps{
                  sh "aws ecs uptade-service --cluster ${CLUSTER_NAME}  --service  ${SERVICE_NAME} --force-newdelpoyment"
           }
        }

  }//sh "aws ecs update-service --cluster ${ECS_CLUSTER_NAME} --service  ${ECS_SERVICE_NAME} --force-new-deployment"