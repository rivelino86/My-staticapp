pipeline {
    agent any

    parameters {
        string(name: 'VERSION', defaultValue: '1.0.0', description: 'App to deploy')
    }

    environment {
        CRED_ID = 'ecr:us-east-1:clinic_app'
        REPO_NAME = 'my-clinicapp'
        REPO_URL = "655040006853.dkr.ecr.us-east-1.amazonaws.com"
        FULL_REPO_URL = "https://${REPO_URL}"
        CLUSTER_NAME = 'clinic-cluster'
        SERVICE_NAME = 'clinic-app-service'
        AWS_REGION = 'us-east-1'
        SONAR_SCANNER= tool 'sonar'
    }

    stages {
         stage('Filesystem Security Scan') {
            steps {
                sh "trivy fs --format table -o clinicapp-scan-report.html ."
            }
        }
        
                      stage('Scan Docker Image with Trivy') {
            steps {
                echo "Scanning Docker image with Trivy"
                sh "trivy image --format table -o docker_image_scan_report_${VERSION}.html ${REPO_URL}/${REPO_NAME}:${VERSION}"
            }
        }
           stage('scan with SonarQube'){
            steps{
                script{
          
             withSonarQubeEnv(credentialsId: 'Sonar_cred') {
                echo "tell me sonar if sonar is ready"
                sh """
                            ${SONAR_SCANNER}/bin/sonar-scanner \
                            -Dsonar.projectKey=clinc_app \
                            -Dsonar.sources=. \
                            -Dsonar.projectName=clinic_app
                            """
     }
   }
}
        }
        stage("Build & Push to ECR") {
            steps {
                script {
                       echo "***********I am a DevOps Engineer**********"
                        sh "docker build -t clinicapp:${params.VERSION} ."      
                        sh "docker tag clinicapp:${params.VERSION} ${REPO_URL}/${REPO_NAME}:${params.VERSION}"
                        sh "docker tag clinicapp:${params.VERSION} ${REPO_URL}/${REPO_NAME}:latest"
            stage('push docker image to ECR'){
                
                    withDockerRegistry(credentialsId: CRED_ID, url: FULL_REPO_URL) {
                     
                        sh "docker push ${REPO_URL}/${REPO_NAME}:latest"
                        sh "docker push ${REPO_URL}/${REPO_NAME}:${params.VERSION}"
                    }
                }
              } 
            }
        }
         stage("Update ECS") {
                steps {
                 sh "aws ecs update-service --cluster ${CLUSTER_NAME} --service  ${SERVICE_NAME} --region ${AWS_REGION} --force-new-deployment"
             }
         }
    }            
}