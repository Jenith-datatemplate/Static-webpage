pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/your-username/static-webpage.git'
            }
        }

        stage('Deploy') {
            steps {
                sh 'cp -r * /var/www/html/'
            }
        }
    }
}
