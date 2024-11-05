pipeline{
    agent any
     

    stages{
        stage('build image & push'){
           steps{
            sh "docker build -t static-app/clinicapp:1.0.0 ."
            //sh "docker tag static_app:latest rivelino86/static_app:latest"
            //sh "docker push rivelino86/static_app:latest"
            //sh "docker rmi static_app"
           }
        }
    }
}