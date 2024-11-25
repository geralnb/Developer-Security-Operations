pipeline {
    agent any
    stages {
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
                recordIssues tools: [xml(path: 'bandit-output.xml', name: 'Bandit')]
            }
        }
    }
}
