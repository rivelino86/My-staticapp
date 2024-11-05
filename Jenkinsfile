pipeline{
    agent any
     

    stages{
        stage('build image'){
           steps{
            sh "docker build -t clinicapp:latest ."
           }
        }
    }
}