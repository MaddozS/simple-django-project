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
        // stage('Preparing MySQL db'){
        //   steps {
        //     script {
        //         def mysqlImage = docker.image('mysql:8.0.15')
        //         def mysqlContainer = mysqlImage.run('--name mysql -e MYSQL_ROOT_PASSWORD=$MYSQL_ROOT_PASSWORD -e MYSQL_DATABASE=$MYSQL_DATABASE -e MYSQL_USER=$MYSQL_USER -e MYSQL_PASSWORD=$MYSQL_PASSWORD -p 3306:3306')
        //     }
        //   }
        // }
        stage('Setup Virtual Environment') {
            steps {
                sh 'python3 -m venv venv'  // Create a virtual environment named 'venv'
                sh 'chmod +x venv/bin/activate'  // Modify the permissions of the activate script
                sh '. venv/bin/activate'  // Activate the virtual environment
            }
        }
        stage('Building') {
            steps {
                // install requirements
                sh 'pip install django'
                sh 'pip install -r requirements.txt'
                sh 'python3 manage.py makemigrations'
                sh 'python3 manage.py migrate'
                // sh 'python3 manage.py rebuild_index'
                sh 'python3 manage.py runserver 0:8001'
                // close server
                sh 'kill $(lsof -t -i:8001)'
            }
        }
        stage('Testing') {
            steps {
                // run tests
                sh 'python3 manage.py test'
            }
        }
        stage('Cleanup') {
            steps {
                sh 'deactivate'  // Deactivate the virtual environment
            }
            // Close docker
            post {
                always {
                    script {
                        mysqlContainer.stop()
                        mysqlContainer.remove()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Success!"'
            }
        }
    }
}