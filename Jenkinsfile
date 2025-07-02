pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/KarimLabidProf/ansible-jenkins-nginx-ssh.git'
            }
        }

        stage('Start Docker container') {
            steps {
                bat 'docker run -d --name ubuntu_nginx -p 2222:22 -p 8080:80 rastasheep/ubuntu-sshd:18.04'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                bat '''
                wsl ansible-playbook -i /mnt/c/ProgramData/Jenkins/.jenkins/workspace/ansible-Test-nginx/inventories/inventory.ini ^
                /mnt/c/ProgramData/Jenkins/.jenkins/workspace/ansible-Test-nginx/playbooks/install-nginx.yml
                '''
            }
        }

        stage('Verify Nginx container') {
            steps {
                bat 'curl http://localhost:8080 || echo "Nginx non disponible"'
            }
        }
    }

    post {
        always {
            echo 'Nettoyage...'
            bat 'docker stop ubuntu_nginx || exit 0'
            bat 'docker rm ubuntu_nginx || exit 0'
        }
    }
}
