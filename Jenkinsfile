pipeline{
    
    agent {label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
                git url:"https://github.com/rks007/two-tier-flask-app.git", branch: "master"
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
        emailtext(
            subject: "Build Success",
            body: "Build Success, good news",
            to: "rishabhkumarsingh94@gmail.com"
        )
    }
    failure{
        emailtext(
            subject: "Build Failed",
            body: "Build Failed, bad news",
            to: "rishabhkumarsingh94@gmail.com"
        )    
    }
}   
}
