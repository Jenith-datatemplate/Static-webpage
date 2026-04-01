pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "jenithdt/static-webpage:${BUILD_NUMBER}"
    }
    parameters {
        choice(
            name: 'ACTION',
            choices: ['build & deploy', 'remove'],
            description: 'Select what you want to do'
        )
    }

    stages {

        stage('Checkout Code') {
            when { expression { params.ACTION == 'build & deploy' } }
            steps {
                git url: 'https://github.com/Jenith-datatemplate/Static-webpage.git',
                    branch: 'main'
            }
        }

        stage('Build Docker Image') {
            when { expression { params.ACTION == 'build & deploy' } }
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Docker Login') {
            when { expression { params.ACTION == 'build & deploy' } }
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-2008',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Docker Push') {
            when { expression { params.ACTION == 'build & deploy' } }
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy Application') {
            when { expression { params.ACTION == 'build & deploy' } }
            steps {
                sh '''
               docker stop nginx-container || true
                docker rm nginx-container || true
                docker run -d -p 8081:80 --name nginx-container jenithdt/static-webpage:latest
                '''
            }
        }

        stage('Remove Application') {
            when { expression { params.ACTION == 'remove' } }
            steps {
                sh ' docker stop nginx-container || true'
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
            echo 'Pipeline completed'
        }
    }
}
