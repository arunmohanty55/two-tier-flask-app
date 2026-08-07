pipeline {

    agent {label "dev"}

    stages {

        stage('code clone') {

            steps {

                echo 'my code'

                git url:"https://github.com/arunmohanty55/two-tier-flask-app.git",
                    branch:"master"
            }
        }
        stage ("File syatem scan"){
            steps {
                sh "trivy fs . -o result.json"
            } 
         }
        stage('build') {

            steps {

                sh "docker build -t flask-app-55 ."

            }
        }


        stage('test') {

            steps {

                echo 'my test'

            }
        }


        stage('push to docker hub') {

            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: "d3fec115-5d4a-4529-8c2a-c7659a7a703f",
                        passwordVariable: "dockerhubpass",
                        usernameVariable: "dockerhubuser"
                    )
                ]) {

                    sh '''
                    docker login -u $dockerhubuser -p $dockerhubpass

                    docker tag flask-app-55 $dockerhubuser/two-tier-flask-app:latest

                    docker push $dockerhubuser/two-tier-flask-app:latest
                    '''
                }
            }
        }


        stage('deploy') {

            steps {

                sh 'docker compose up -d'

            }
        }

    }
}
