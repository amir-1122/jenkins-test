pipeline {
    agent any

    environment {
            VERCEL_TOKEN = credentials('vercel_token')
    }

    stages {
        stage('install') {
            setps {
                bat 'npm install'
            }
        }
        stage('Test') {
            setps {
                echo 'Skipping tests - no test script found'
            }
        }
        stage('Build') {
            setps {
                bat 'npm run build'
            }
        }  
        stage('Deploy') {
            setps {
                bat 'npx vercel --prod --yes --token=%VERCEL_TOKEN%'
            }
        }  
    }
}