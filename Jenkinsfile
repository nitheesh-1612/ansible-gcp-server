pipeline {
    agent any
    environment {
        ANSIBLE_SERVER = "34.73.99.65"
    }
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo "copying all neccessary files to ansible control node"
                    sshagent(['ansible-server']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@${ANSIBLE_SERVER}:/root"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ansible-client-server', keyFileVariable: 'keyfile', usernameVariable: 'ansible')]) {
                            sh 'scp $keyfile root@$ANSIBLE_SERVER:/root/ssh-key.pem'
                        }
                    }
                }
            }
        }
        stage("execute ansible playbook") {
            steps {
                script {
                    echo "calling ansible playbook to configure ec2 instances"
                    def remote = [:]
                    remote.name = "ansible"
                    remote.host = ANSIBLE_SERVER
                    remote.allowAnyHosts = true

                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server', keyFileVariable: 'keyfile', usernameVariable: 'jenkins')]){
                        remote.user = jenkins
                        remote.identityFile = keyfile
                        // sshScript remote: remote, script: "prepare-ansible-server.sh"
                        echo "checking whether entered sudo or not"
                        sshCommand remote: remote, command: "ls -l"
                        // sh 'ansible-playbook playbook.yaml'
                    }
                }
            }
        }
    }   
}
