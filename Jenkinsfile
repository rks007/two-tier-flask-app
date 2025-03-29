@Library("shared") _
pipeline{
    
    agent {label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
                script{
                    clone("https://github.com/rks007/two-tier-flask-app.git","master")
                }
            }
        }
        stage("Trivy File System Scan"){
            steps{
                script{
                    trivy_fs();
                }
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
                script{
                    docker_push(dockerHubCreds, two-tier-flask-app);
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
