pipeline {
    agent any
    stages {
        stage('Clone repository') {
            steps {
                git credentialsId: 'token-github', url: 'https://github.com/XAVNONO/python-api-handle-it.git'
            }
        }
        
        stage('Build Pylint image') {
            steps {
                script {
                    DockerImage1 = docker.build("xavnono/mypylint:latest","-f docker-test/pylint/Dockerfile docker-test/pylint/")
                }
            }
        }
        
        stage('Push pylint image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'reg2', url: 'https://hub.docker.com/repository/docker/xavnono/image_python-api-handle-it') {
                    dockerImage1.push()
                    }
                }
            }
        }
        
        stage ('Pylint'){
            agent {
                docker {
                    image 'xavnono/mypylint'
                    args '-v ${PWD}:/app'
                    reuseNode true
                }
            }
            steps {
                sh 'pylint --recursive yes --exit-zero app '
            }
        }
        
        stage('Build Unittest image') {
            steps {
                script {
                    DockerImage2 = docker.build("xavnono/myunittest:latest","-f docker-test/unittest/Dockerfile docker-test/unittest/")
                }
            }
        }
        
        stage('Push unittest image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'reg2', url: 'https://hub.docker.com/repository/docker/xavnono/image_python-api-handle-it') {
                    dockerImage2.push()
                    }
                }
            }
        }
        
        stage ('Unit tests'){
            agent {
                docker {
                    image 'vanessakovalsky/myunittest'
                    args '-v ${PWD}:/app'
                    reuseNode true
                }
            }
            steps {
                sh 'cd app; python -m unittest test/unit/test.py '
            }
        }
       
        
        stage('Build image') {
            steps {
                script {
                    DockerImage3 = docker.build("xavnono/mypythonapp:latest","-f docker-app/python/Dockerfile .")
                }
            }
        }
         stage('Push image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'reg2', url: 'https://hub.docker.com/repository/docker/xavnono/image_python-api-handle-it') {
                    dockerImage3.push()
                    }
                }
            }
         }
    }
}  
