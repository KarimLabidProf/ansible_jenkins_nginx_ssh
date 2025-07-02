pipeline {
    agent any
    environment {
        CONTAINER_NAME = 'ubuntu_nginx'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/KarimLabidProf/ansible_jenkins_nginx_ssh.git', branch: 'main'
            }
        }

        stage('Start Docker container') {
            steps {
                bat 'docker run -d --name %CONTAINER_NAME% -p 2222:22 rastasheep/ubuntu-sshd:18.04'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                bat '''
                    wsl ansible-playbook -i /mnt/c/ProgramData/Jenkins/.jenkins/workspace/${JOB_NAME}/inventories/inventory.ini \
                    /mnt/c/ProgramData/Jenkins/.jenkins/workspace/${JOB_NAME}/playbooks/install-nginx.yml
                '''
            }
        }

        stage('Verify Nginx container') {
            steps {
                bat 'curl http://localhost:8080 || exit 1'
            }
        }
    }

    post {
        always {
            echo 'Nettoyage...'
            bat 'docker stop %CONTAINER_NAME% || exit 0'
            bat 'docker rm %CONTAINER_NAME% || exit 0'
        }
    }
}
