node{

    def registryProjet='registry.gitlab.com/xavnono/python-api-handle-it' 
    def IMAGE="${registryProjet}:version-${env.BUILD_ID}"

        stage('Clone repository') {
                git credentialsId: 'token-github', url: 'https://github.com/XAVNONO/python-api-handle-it.git'
            }

    def DockerImage1 = stage('Build Pylint image') {   
        docker.build("$IMAGE","-f docker-test/pylint/Dockerfile docker-test/pylint/")
                }
        
        stage('Push pylint image') {
                    docker.withRegistry('https://registry.gitlab.com', 'reg1') {
                    DockerImage1.push 'latest'
                    DockerImage1.push()
            }
        }

        stage ('Pylint'){
            agent {
                docker {
                    image "xavnono/python-api-handle-it"
                    args '-v ${PWD}:/app'
                    reuseNode true
                }
            }
            steps {
                sh 'mkdir -p app/reports/pylint; pylint --output-format json --recursive yes --exit-zero app > app/reports/pylint/report.json;pylint-json2html -o app/reports/pylint/report.html app/reports/pylint/report.json'
            }
            }
        }
    }