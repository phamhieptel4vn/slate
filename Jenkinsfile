pipeline {
    agent any
    stages {
        stage('SSH Remote') {
            steps {
                script {
                    def remote = [:]
                    remote.name = 'server-dev'
                    remote.host = '113.164.246.25'
                    remote.port = 8822
                    remote.allowAnyHosts = true
                    withCredentials([sshUserPrivateKey(credentialsId: 'root-tel4vn-2019', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
                        remote.user = userName
                        remote.identityFile = identity
                        stage("Pull") {
                            sshCommand remote: remote, command: 'cd /root/projects/slate && git pull'
                        }
                        stage("Build") {
                            sshCommand remote: remote, command: 'cd /root/projects/slate && docker run --rm --name slate -v $(pwd)/build:/srv/slate/build -v $(pwd)/source:/srv/slate/source slatedocs/slate'
                        }
                        stage("Deploy") {
                            sshCommand remote: remote, command: 'cd /root/projects/slate && docker-compose up -d'
                        }
                    }
                }
            }
        }
    }
}
