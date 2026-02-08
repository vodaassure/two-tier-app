pipeline {
    agent { label 'dev' }

    stages {

        stage("code") {
            steps {
                git branch: "main", url: "https://github.com/vodaassure/two-tier-app.git"
                echo "code clone done"
            
            }
        }

        stage("Build") {
            steps {
                sh " docker build -t two-tier-flask-app ."
                echo "building is done"
            }
        }

        stage("Test") {
            steps {
                echo "Testing"
            }
        }

        stage("push to docker hub"){
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerhub",
                passwordVariable: "dockerhubPass",
                usernameVariable: "dockerhubUser"
                )]){

                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                sh " docker tag two-tier-flask-app ${env.dockerhubUser}/two-tier-flask-app"
                sh " docker push ${env.dockerhubUser}/two-tier-flask-app:latest"

                }

            }
        }

        stage("deploy") {
            steps {
                echo "deploying"
                sh "docker compose up -d"
            }
        }

    }
}
