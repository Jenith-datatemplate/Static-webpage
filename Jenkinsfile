pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "jenithjs/static-webpage:v1"
    }
    parameters {
        choice(
            name: 'ACTION',
            choices: ['build', 'deploy', 'remove'],
            description: 'Select what you want to do'
        )
    }

    stages {

        stage('Checkout Code') {
            when { expression { params.ACTION == 'build' } }
            steps {
                git url: 'https://github.com/Jenith-datatemplate/Static-webpage.git',
                    branch: 'master'
            }
        }

        stage('Build Docker Image') {
            when { expression { params.ACTION == 'build' } }
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Docker Login') {
            when { expression { params.ACTION == 'build' } }
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
            when { expression { params.ACTION == 'build' } }
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy Application') {
            when { expression { params.ACTION == 'deploy' } }
            steps {
                sh '''
               docker stop nginx-container || true
                docker rm nginx-container || true
                docker run -d -p 8080:80 --name nginx-container $DOCKER_IMAGE
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
