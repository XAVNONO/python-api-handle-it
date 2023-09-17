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
                    withDockerRegistry(credentialsId: 'reg2', url: '') {
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
                sh 'mkdir -p app/reports/pylint; pylint --output-format json --recursive yes --exit-zero app > app/reports/pylint/report.json;pylint-json2html -o app/reports/pylint/report.html app/reports/pylint/report.json'
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
                    withDockerRegistry(credentialsId: 'reg2', url: '') {
                    dockerImage2.push()
                    }
                }
            }
        }
        
        stage ('Unit tests'){
            agent {
                docker {
                    image 'xavnono/myunittest'
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
                    withDockerRegistry(credentialsId: 'reg2', url: '') {
                    dockerImage3.push()
                    }
                }
            }
         }
    }
}  
