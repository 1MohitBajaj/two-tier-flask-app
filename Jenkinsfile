pipeline{
    
    agent { label "dev"};
    stages{
        stage("code"){
            steps{
                git url:"https://github.com/1MohitBajaj/two-tier-flask-app.git", branch:"master"
                echo "code clonne ho gaya h"
            }
        }
        stage("build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
            
        }
        stage("test"){
            steps{
                echo "developer/tester tests likh ke dega.."
            }
            
        }
        
        stage("Push to docker hub"){
            steps{
                withCredentials([usernamePassword(
                credentialsId:"dockerhubcreds",
                passwordVariable: "dockerhubpass",
                usernameVariable: "dockerhubuser"
                )]){
                    sh "docker login -u $dockerhubuser -p $dockerhubpass "
                    sh "docker image tag two-tier-flask-app   $dockerhubuser/two-tier-flask-app"
                    sh "docker push $dockerhubuser/two-tier-flask-app:latest"
                }
                
            }
        }
        stage("deploy"){
            steps{
            sh "docker-compose up -d --build flask-app "
            }
        }
    }

post {
    success{
        script {
            emailtext from: "mohitknight956@gmail.com",
            subject: "Demo app has been updated and deployed successully",
            body: "Build Successful!",
          to: 'mohitknight956@gmail.com',
          mimeType: 'text/html'
        }
    }
    failure {
        script {
            emailtext from: "mohitknight956@gmail.com",
            subject: "Demo app has been updated but it failed ",
            body: "Build Successful!",
          to: 'mohitknight956@gmail.com',
          mimeType: 'text/html'
        }
    }
}  
} 
