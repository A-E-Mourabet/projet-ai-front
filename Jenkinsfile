pipeline {
    agent any

    environment {
        VERCEL_TOKEN = credentials('vercel-token') // Replace 'vercel-token' with your Vercel API token ID
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Use NodeJS Plugin
                    nodejs('nodejs-setup') { // Replace 'nodejs-setup' with your NodeJS installation name in Jenkins
                        sh 'npm install'
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    nodejs('nodejs-setup') {
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Build Project') {
            steps {
                script {
                    nodejs('nodejs-setup') {
                        sh 'npm run build'
                    }
                }
            }
        }

        stage('Deploy to Vercel') {
            steps {
                script {
                    sh '''
                    # Deploy to Vercel using CLI
                    npm install -g vercel
                    vercel --token=$VERCEL_TOKEN --prod
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed!'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
