pipeline{
    agent any
     

    stages{
        stage('build image'){
           steps{
            sh "docker build -t static_app/clinicapp:latest ."
            sh "docker push rivelino86/static_app:latest"
            sh "docker rmi static_app"
           }
        }
    }
}