pipeline {

    agent { label "dev" }

    stages {

        stage('Code Clone') {
            steps {
                echo 'Cloning code'
                git url: "https://github.com/arunmohanty55/two-tier-flask-app.git",
                    branch: "master"
            }
        }

        stage('File System Scan') {
            steps {
                sh "trivy fs --format json -o result.json ."
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t flask-app-55 ."
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
            }
        }

        stage('Push to Docker Hub') {
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

        stage('Deploy') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }

    post {

        success {
            script {
                emailext (
                    from: 'arunmohanty9535@gmail.com',
                    to: 'arunmohanty9535@gmail.com',
                    subject: 'Build Successful ✅',
                    body: 'Good news: Your build was successful 🎉'
                )
            }
        }

        failure {
            script {
                emailext (
                    from: 'arunmohanty9535@gmail.com',
                    to: 'arunmohanty9535@gmail.com',
                    subject: 'Build Failed ❌',
                    body: 'Alert: Your build failed. Please check Jenkins logs 🚨'
                )
            }
        }
    }
}
