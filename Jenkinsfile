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
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@${ANSIBLE_SERVER}:/home/nitheesh_r_varma"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ansible-client-server', keyFileVariable: 'keyfile', usernameVariable: 'ansible')]) {
                            sh 'scp $keyfile root@$ANSIBLE_SERVER:/home/nitheesh_r_varma/ssh-key.pem'
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

                node {
            stage('Execute Remote Commands') {
            withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server', keyFileVariable: 'SSH_KEY')]) {
            def remoteServer = '34.73.99.65' // Replace with the IP or hostname of your remote server
            def remoteUser = 'root' // Replace with the username of your remote server
            def sshCommand = "ssh -i ${env.SSH_KEY} ${remoteUser}@${remoteServer}"
            
            // Execute remote commands
                // sh "cd /root"
                sh "pwd"
                sh "ls -l"
                // sh "ansible-playbook playbook.yaml"
            
            // Add more commands as needed
                                }
                            }
                        }

                    // withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server', keyFileVariable: 'keyfile')]){
                    //     def remoteServer = ANSIBLE_SERVER 
                    //     def remoteUser = 'sudo'
                    //     def sshCommand = "ssh -i ${env.keyfile} ${remoteUser}@${remoteServer}"
                    //     // remote.user = 'sudo'
                    //     // remote.identityFile = keyfile
                    //     // sshScript remote: remote, script: "prepare-ansible-server.sh"
                    //     echo "checking whether entered sudo or not"
                    //     sshCommand remote: remote, command: "ls -l"
                    //     // sh 'ansible-playbook playbook.yaml'
                //     }
                }
            }
        }
    }   
}
