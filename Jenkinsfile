pipeline {
    agent any

    environment {
        // Define environment variables for your GitHub and Vercel details
        GITHUB_REPO = 'https://github.com/A-E-Mourabet/projet-ai-front.git'
        VERCEL_TOKEN = credentials('vercel-key') // Store Vercel token as a Jenkins credential
        VERCEL_PROJECT_NAME = 'projet-ai-front-11'
       //VERCEL_PROJECT_ID = credentials('VERCEL_PROJECT_ID')
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
                    // If it's an Angular app, install dependencies using npm
                    if (fileExists('package.json')) {
                        bat 'npm install'
                    }
                }
            }
        }

        stage('Build Angular Project') {
            steps {
                script {
                    // Build the Angular project using Angular CLI
                    if (fileExists('angular.json')) {
                        bat 'npm run build --prod'
                    }
                }
            }
        }

        stage('Deploy to Vercel') {
            steps {
                script {
                    // Authenticate with Vercel using the Vercel token
                  withCredentials([string(credentialsId: 'vercel-key', variable: 'vercel-key')]) {
                        // Deploy the project to Vercel
                    bat"""
                     cd dist\\front-end
                     vercel --token %VERCEL_TOKEN% --prod --yes
                    """
                        //bat "vercel --token %VERCEL_TOKEN% --prod --yes --cwd dist/front-end "
                    }
                    //bat 'vercel login --token %VERCEL_TOKEN%'
                    
                    // Deploy to Vercel (this will trigger the Vercel deployment)
                    //bat 'vercel --prod --token %VERCEL_TOKEN% --confirm --scope %VERCEL_ORG_ID% --project %VERCEL_PROJECT_NAME%'
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
