pipeline {
    agent any

    environment {    
        DOCKERHUB_CREDENTIALS = credentials('dockerid')
    }
        
    stages {
        stage('SCM Checkout') {
            steps {
                echo 'Perform SCM Checkout'
                git 'https://github.com/parida-123/BankingApp.git'
            }
        }
        stage('Application Build') {
            steps {
                echo 'Perform Application Build'
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                echo 'Perform Docker Build'
                sh "docker build -t 8984439511/bank-app:${BUILD_NUMBER} ."
                sh "docker tag 8984439511/bank-app:${BUILD_NUMBER} 8984439511/bank-app:latest"
                sh 'docker image list'
            }
        }
        stage('Login to Dockerhub') {
            steps {
                echo 'Login to DockerHub'                
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Publish the Image to Dockerhub') {
            steps {
                echo 'Publish to DockerHub'
                sh "docker push 8984439511/bank-app:${BUILD_NUMBER}"                
                sh "docker push 8984439511/bank-app:latest"
            }
        }

        stage('Execute Ansible Playbook') {
            steps {
                script {
                    ansiblePlaybook become: true, credentialsId: 'cred-ansible', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
                }
            }
        }
    }
    
    post {
        failure {
            echo 'Send mail on failure'
            mail bcc: 'dibyaparida77355@gmail.com', body: 'banking webapp', cc: 'dibyaparida77355@gmail.com', from: '', replyTo: '', subject: 'banking app build successfully', to: 'dibyaparida77355@gmail.com'
        }
        success {
            echo 'Send mail on success'
            mail bcc: 'dibyaparida77355@gmail.com', body: 'banking webapp', cc: 'dibyaparida77355@gmail.com', from: '', replyTo: '', subject: 'banking app build unsuccessfully', to: 'dibyaparida77355@gmail.com'
        }
    }
}
