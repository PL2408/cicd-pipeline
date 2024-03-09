pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {
                // Завантаження коду з репозиторію Git
                git 'https://github.com/PL2408/cicd-pipeline.git'
            }
        }
        stage('build') {
            steps {
                // Крок збирання проекту за допомогою скрипта build.sh
                sh './cicd-pipeline/scripts/build.sh'
            }
        }
        stage('test') {
            steps {
                // Крок запуску тестів за допомогою скрипта test.sh
                sh './cicd-pipeline/scripts/test.sh'
            }
        }
        stage('build_docker_image') {
            steps {
                // Крок створення Docker-образу за допомогою Dockerfile
                sh 'docker build -t myapp ./cicd-pipeline'
            }
        }
        stage('deploy') {
            steps {
                // Крок розгортання Docker-образу на різних портах в залежності від гілки
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'docker run -d -p 3000:3000 myapp'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'docker run -d -p 3001:3000 myapp'
                    } else {
                        echo 'Unsupported branch'
                    }
                }
            }
        }
    }
}

