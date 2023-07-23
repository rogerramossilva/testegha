pipeline {
    agent any

    stages {
        stage('Deploy with Ansible') {
            steps {
                sh 'ansible-playbook -i ansible/hosts ansible/installweb.yml'
            }
        }
    }
}

