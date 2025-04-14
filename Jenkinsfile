pipeline {
    agent any

    environment {
        EC2_IP = '13.60.197.47'
        EC2_USER = 'ubuntu'
        SSH_KEY_PATH = 'C:/Users/sasha/Downloads/key.pem'
        REMOTE_DIR = '/home/ubuntu/app'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/OleksandrPoD/javatestapp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh """
                    scp -i ${SSH_KEY_PATH} target/javatestapp.jar ${EC2_USER}@${EC2_IP}:${REMOTE_DIR}/
                """
            }
        }

        stage('Run Application on EC2') {
            steps {
                sh """
                    ssh -i ${SSH_KEY_PATH} ${EC2_USER}@${EC2_IP} 'java -jar ${REMOTE_DIR}/javatestapp.jar &'
                """
            }
        }
    }
}

