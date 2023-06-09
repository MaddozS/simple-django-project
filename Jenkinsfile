pipeline {
    agent any
    environment {
      MYSQL_ROOT_PASSWORD = credentials('MYSQL_ROOT_PASSWORD')
      MYSQL_DATABASE = credentials('MYSQL_DATABASE')
      MYSQL_USER = credentials('MYSQL_USER')
      MYSQL_PASSWORD = credentials('MYSQL_PASSWORD')
    }
    stages {
        stage('Cloning repo') {
            steps {
                git credentialsId: 'github-axel-anaya-ssh', url: 'git@github.com:MaddozS/simple-django-project.git'
            }
        }
        stage('Building') {
            steps {
                withPythonEnv('miobra') {
                    sh 'which python'
                    sh 'pip install -r requirements.txt'
                    sh 'python3 manage.py makemigrations'
                    sh 'python3 manage.py migrate'
                }
                // install requirements
            }
        }
        stage('Testing') {
            steps {
                withPythonEnv('miobra') {
                    sh 'python3 manage.py test'
                    sh 'echo "Everything is OK!"'
                }
                // run tests
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Success!"'
            }
        }
    }
}
