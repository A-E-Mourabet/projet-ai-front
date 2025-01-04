pipeline {
    agent any

    environment {
        // Define environment variables for your GitHub and Vercel details
        GITHUB_REPO = 'https://github.com/your-username/your-repository.git'
        VERCEL_TOKEN = credentials('vercel-key') // Store Vercel token as a Jenkins credential
        VERCEL_PROJECT_NAME = 'your-vercel-project'
        VERCEL_ORG_ID = 'your-vercel-org-id' // Optional if needed
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your GitHub repository
                git branch: 'main', url: "${GITHUB_REPO}"
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // If it's a Node.js app, install dependencies using npm
                    if (fileExists('package.json')) {
                        sh 'npm install'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build your project if necessary (e.g., for Angular, React, etc.)
                    // For example, for a React app:
                    if (fileExists('package.json')) {
                        sh 'npm run build'
                    }
                }
            }
        }

        stage('Deploy to Vercel') {
            steps {
                script {
                    // Authenticate with Vercel using the Vercel token
                    sh 'vercel login --token $VERCEL_TOKEN'
                    
                    // Deploy to Vercel (this will trigger the Vercel deployment)
                    sh 'vercel --prod --token $VERCEL_TOKEN --confirm --scope $VERCEL_ORG_ID --project $VERCEL_PROJECT_NAME'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment to Vercel was successful!'
        }
        failure {
            echo 'Deployment to Vercel failed.'
        }
    }
}
