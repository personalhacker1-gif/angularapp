pipeline {
    agent any
    
    environment {
        // Jenkins Credentials mein se token utha kar variable mein save karein
        VERCEL_TOKEN = credentials('VERCEL_TOKEN')
    }

    stages {
        // STEP 1: Angular App ka production build taiyar karna
        stage('Build') {
            steps {
                echo 'Starting Node Modules Installation and Production Build...'
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        // STEP 2: Build folder ko Vercel par deploy karna (Ubuntu compatible)
        stage('Deploy') {
            steps {
                echo 'Deploying to Vercel Production...'
                
                /* 
                  Ubuntu/Linux mein variable ke liye "${VERCEL_TOKEN}" double quotes ke sath use karein.
                  --yes flag lagana zaroori hai taaki Vercel continuous integration (CI) ko bina confirmation prompt ke automatic deploy karne de.
                */
                sh "npx vercel --prod --token ${VERCEL_TOKEN} --yes"
            }
        }
    }
}