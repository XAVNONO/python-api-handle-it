pipeline {

        environment {
        registry = "xavnono/mypylint"
        registry2 = "xavnono/myunittest"
        registryCredential = 'dockerhub'
        dockerImage = ''
        dockerImage2 = ''
        }

    agent any
    stages {
        stage('Clone Github') {
            steps {
                git credentialsId: 'token-github', url: 'https://github.com/XAVNONO/python-api-handle-it.git'
            }
        }
        
        stage('Build Pylint image') {
            steps {
                script {
                    dockerImage = docker.build (registry + ":$BUILD_NUMBER","-f docker-test/pylint/Dockerfile docker-test/pylint/")
                }
            }
        }
        
        stage('Push pylint image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
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
                    dockerImage2 = docker.build (registry2 + ":$BUILD_NUMBER","-f docker-test/unittest/Dockerfile docker-test/unittest/")
                }
            }
        }
        
        stage('Push unittest image') {
            steps {
                script {
                    docker.withRegistry2( '', registryCredential ) {
                    dockerImage2.push()
                    }
                }
            }
        }
        
        stage ('Unittests'){
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
                    DockerImage = docker.build("xavnono/mypythonapp:latest","-f docker-app/python/Dockerfile .")
                }
            }
        }
         stage('Push image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'reg2', url: '') {
                    dockerImage.push()
                    }
                }
            }
         }
    }
}  
