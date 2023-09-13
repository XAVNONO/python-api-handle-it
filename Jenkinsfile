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

    def registryProjet='registry.gitlab.com/xavnono/presentations-jenkins' 
    def IMAGE="${registryProjet}:version-${env.BUILD_ID}"
        
        stage('Push pylint image') {
        docker.withRegistry('https://registry.gitlab.com', 'reg1') {
            img.push 'latest'
            img.push()
            }
        }
    }
}
