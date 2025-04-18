pipeline {
    agent any
    environment {
        LOCAL_HOME = "/home/sakthi/py_pro/"
        REMOTE_IP = 'test.domain.com'
        REMOTE_USER = 'sakthi'
        REMOTE_PASSWORD = 'Sathish@1433'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/sathishyadava143/python_app_cicd.git'
            }
        }
        stage("Copy Files to Remote") {
            steps {
                sshagent(['e4294084-dcc1-4af3-a3e9-cac203e3cd7e']){
                   sshGet remote: [
                        name: 'test',
                        host: REMOTE_IP,
                        user: REMOTE_USER,
                        password: REMOTE_PASSWORD,
                        fileTransfer: 'scp',
                        allowAnyHosts: true
                    ], from: "${WORKSPACE}/", filterRegex: ".+\\.(log|csv)\$", into: LOCAL_HOME, override: true
                }
            }
        }
        stage("Docker Build") {
            steps {
                sh "cd ${LOCAL_HOME} && docker build -t app_py ."
            }
        }
        stage('Docker Deploy') {
            steps {
                sh "docker rm -f python-app || true && docker run -dit --name python-app --network host app_py"
            }
        }
    }
}
