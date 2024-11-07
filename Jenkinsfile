pipeline{
    agent any

     parameters {
        string (name: 'VERSION', defaultValue: '1.0.0')
     }
    stages{
        stage('build image & push '){
           steps{
            script{
                // This step should not normally be used in your script. Consult the inline help for details.
                   withDockerRegistry(credentialsId: 'dockerhub_id') {
                    sh "docker build -t clinicapp:${params.VERSION} ."
                    sh "docker tag clinicapp:${params.VERSION} rivelino86/clinicapp:${params.VERSION}"
                    sh "docker push rivelino86/clinicapp:${params.VERSION}"
               }
             }
           
           }
        }
        stage("Build & push to ECR"){

        
        steps{
            script{
                // This step should not normally be used in your script. Consult the inline help for details.
                 withDockerRegistry(credentialsId: 'ecr:us-east-1:clinic_app', url: 'https://655040006853.dkr.ecr.us-east-1.amazonaws.com/'){
                        echo ' "***********I am a DevOps Engineer*********" '

                        sh "docker build -t clinicapp:${params.VERSION} ."

                        sh "docker tag clinicapp:${params.VERSION} 655040006853.dkr.ecr.us-east-1.amazonaws.com/my-clinicapp:${params.VERSION}"

                        sh "docker tag clinicapp:${params.VERSION} 655040006853.dkr.ecr.us-east-1.amazonaws.com/my-clinicapp:latest"

                        sh "docker push 655040006853.dkr.ecr.us-east-1.amazonaws.com/my-clinicapp:latest"

                        sh "docker push 655040006853.dkr.ecr.us-east-1.amazonaws.com/my-clinicapp:${params.VERSION}"
                 }
            }
        }
    }
  }
}