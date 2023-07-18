pipeline {
    agent any
    environment {
        ANSIBLE_SERVER = "52.91.167.174"
    }
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo "copying all neccessary files to ansible control node"
                    sshagent(['ansible-server']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@${ANSIBLE_SERVER}:/home/ubuntu"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ansible-client-server', keyFileVariable: 'keyfile', usernameVariable: 'ansible')]) {
                            sh 'scp $keyfile root@$ANSIBLE_SERVER:/home/ubuntu/ssh-key.pem'
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

                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server', keyFileVariable: 'keyfile', usernameVariable: 'ansible')]){
                        //remote.user = ansible
                        //remote.identityFile = keyfile
                        // sshScript remote: remote, script: "prepare-ansible-server.sh"
                        // echo "checking whether entered sudo or not"
                        // sshCommand remote: remote, command: "ansible-playbook playbook.yaml"
                        sh 'ansible-playbook playbook.yaml'
                    }
                }
            }
        }
    }   
}
