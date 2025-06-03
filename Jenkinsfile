pipeline {
    environment {
        IMAGEN = "madand1/django_tutorial"
        LOGIN = "dockerhub-madand1"
    }
    agent none

    stages {
        stage("Desarrollo") {
            agent {
                docker {
                    image "python:3"
                    args "-u root:root"
                }
            }
            stages {
                stage("Clone") {
                    steps {
                        git branch: 'master', url: 'https://github.com/madand1/django_tutorial.git'
                    }
                }
                stage("Install") {
                    steps {
                        sh 'pip install -r requirements.txt'
                    }
                }
                stage("Test") {
                    steps {
                        sh 'python3 manage.py test'
                    }
                }
            }
        }

        stage("Construccion") {
            agent any
            stages {
                stage("CloneAnfitrion") {
                    steps {
                        git branch: 'master', url: 'https://github.com/madand1/django_tutorial.git'
                    }
                }
                stage("BuildImage") {
                    steps {
                        script {
                            newApp = docker.build("${IMAGEN}:latest")
                        }
                    }
                }
                stage("UploadImage") {
                    steps {
                        script {
                            docker.withRegistry('', LOGIN) {
                                newApp.push()
                            }
                        }
                    }
                }
                stage("RemoveImage") {
                    steps {
                        sh "docker rmi ${IMAGEN}:latest"
                    }
                }
            }
        }
    }
}
