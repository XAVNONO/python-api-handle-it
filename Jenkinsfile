pipeline {

    //>>>>>Définition projet et versionning image
    def registryProjet='registry.gitlab.com/xavnono/presentations-jenkins/TP3' 
    def IMAGE="${registryProjet}:version-${env.BUILD_ID}"

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
                    dockerImage = docker.build("xavnono/mypylint:latest","-f docker-test/pylint/Dockerfile docker-test/pylint/")
                }
            }
        }
        
        stage('Push pylint image') {
            docker.withRegistry('https://registry.gitlab.com', 'reg1') {
                dockerImage.push 'latest'
                dockerImage.push()
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
                    dockerImage2 = docker.build("xavnono/myunittest:latest","-f docker-test/unittest/Dockerfile docker-test/unittest/")
                }
            }
        }
        
        stage('Push unittest image') {
            docker.withRegistry('https://registry.gitlab.com', 'reg1') {
                dockerImage2.push 'latest'
                dockerImage2.push()
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
                    dockerImage3 = docker.build("xavnono/mypythonapp:latest","-f docker-app/python/Dockerfile .")
                }
            }
        }
         stage('Push image') {
            docker.withRegistry('https://registry.gitlab.com', 'reg1') {
                dockerImage3.push 'latest'
                dockerImage3.push()
                    }
        }
    }
}  
