node{

    //>>>>>Définition projet et versionning image
    def registryProjet='registry.gitlab.com/xavnono/presentations-jenkins/TP3' 
    def DockerImage="${registryProjet}:version-${env.BUILD_ID}"
    def DockerImage2="${registryProjet}:version-${env.BUILD_ID}"
    def DockerImage3="${registryProjet}:version-${env.BUILD_ID}"

    stages {
        stage('Clone repository') {
            steps {
                git credentialsId: 'token-github', url: 'https://github.com/XAVNONO/python-api-handle-it.git'
            }
        }
        
        stage('Build Pylint image') {
            steps {
                script {
                    DockerImage = docker.build("xavnono/mypylint:latest","-f docker-test/pylint/Dockerfile docker-test/pylint/")
                }
            }
        }
        
        stage('Push pylint image') {
            docker.withRegistry('https://registry.gitlab.com', 'reg1') {
                DockerImage.push 'latest'
                DockerImage.push()
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
            docker.withRegistry('https://registry.gitlab.com', 'reg1') {
                DockerImage2.push 'latest'
                DockerImage2.push()
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
            docker.withRegistry('https://registry.gitlab.com', 'reg1') {
                DockerImage3.push 'latest'
                DockerImage3.push()
                    }
        }
    }
}  
