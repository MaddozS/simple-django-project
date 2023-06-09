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
        stage('Setup Virtual Environment') {
            steps {
                sh 'python3 -m venv miobra'  // Create a virtual environment named 'venv'
                sh 'chmod +x venv/bin/activate'  // Modify the permissions of the activate script
                sh '. venv/bin/activate'  // Activate the virtual environment
                sh 'which python'
            }
        }
        stage('Building') {
            steps {
                // install requirements
                sh '. miobra/bin/activate'  // Activate the virtual environment
                sh 'which python'
                sh 'pip install -r requirements.txt'
                sh 'python3 manage.py makemigrations'
                sh 'python3 manage.py migrate'
            }
        }
        stage('Testing') {
            steps {
                // run tests
                sh '. miobra/bin/activate'  // Activate the virtual environment
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
