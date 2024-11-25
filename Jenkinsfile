pipeline {
    agent any
    environment {
        PATH = "/usr/bin:/usr/local/bin:${env.PATH}"
    }
    stages {
        stage('Setup Python') {
            steps {
                sh 'python3 --version'
                sh 'pip3 --version'
            }
        }
        stage('Checkout') {
            steps {
                git url: 'https://github.com/geralnb/Developer-Security-Operations.git', branch: 'main'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install bandit
                '''
            }
        }
        stage('SAST Analysis') {
            steps {
                sh '''
                    . venv/bin/activate
                    bandit -f xml -o bandit-output.xml -r . || true
                '''
                script {
                    recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
                }
            }
        }
    }
}
