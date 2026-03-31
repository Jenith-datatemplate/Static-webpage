pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/Jenith-datatemplate/Static-webpage.git'
            }
        }

        stage('Deploy') {
            steps {
                sh 'cp -r * /var/www/html/'
            }
        }
    }
}
