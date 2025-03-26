pipeline{
    
    agent {label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
                git url:"https://github.com/rks007/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Trivy File System Scan"){
            steps{
                sh "trivy fs . -o results.json"
            }
        }    
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Developer/tester test lik ke dega"
            }
        }
        stage("push to docker hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]){
                    
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }

post {
    success{
        emailext body: "Build Success",
        subject: "Build Success",
        to: "rishabhkumarsingh94@gmail.com"
    }
    failure{
        emailext body: "Build Failed",
        subject: "Build Failed",
        to: "rishabhkumarsingh94@gmail.com"  
    }
}   
}
