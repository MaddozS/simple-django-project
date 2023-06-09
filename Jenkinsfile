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
        withPythonEnv('miobra') {
            stage('Building') {
                steps {
                    // install requirements
                    sh 'which python'
                    sh 'pip install -r requirements.txt'
                    sh 'python3 manage.py makemigrations'
                    sh 'python3 manage.py migrate'
                }
            }
            stage('Testing') {
                steps {
                    // run tests
                    sh 'python3 manage.py test'
                    sh 'echo "Everything is OK!"'
                }
            }
            stage('Deploy') {
                steps {
                    sh 'echo "Success!"'
                }
            }
        }
    }
}
