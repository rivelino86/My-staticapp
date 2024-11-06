pipeline{
    agent any
     

    stages{
        stage('build image & push'){
           steps{
            script{
                // This step should not normally be used in your script. Consult the inline help for details.
                   withDockerRegistry(credentialsId: 'dockerhub_id') {
                    sh "docker build -t clinicapp:1.0.0 ."
                    sh "docker tag clinicapp:1.0.0 rivelino86/clinicapp1.0.0"
                    sh "docker push rivelino86/clinicapp:1.0.0"
               }
             }
           
           }
        }
    }
}