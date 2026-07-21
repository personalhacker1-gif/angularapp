pipeline {
    agent any
    
    tools {
        nodejs 'node20'
    }
    
    
    environment {
        DEPLOY_DIR = '/var/www/myapp'
        PROJECT_NAME = 'project' 
        
        // 📧 Multi-recipients separated by comma and space
        EMAIL_RECIPIENTS = 'afridisalmankhan@gmail.com, 000sultankhan@gmail.com, personalhacker1@gmail.com'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Installing dependencies and building Angular app...'
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        stage('Deploy to Nginx') {
            steps {
                echo 'Deploying Angular build to Nginx...'
                sh "sudo mkdir -p ${DEPLOY_DIR}"
                sh "sudo rm -rf ${DEPLOY_DIR}/*"
                sh "sudo cp -r dist/${PROJECT_NAME}/browser/* ${DEPLOY_DIR}/ || sudo cp -r dist/${PROJECT_NAME}/* ${DEPLOY_DIR}/"
                sh "sudo chown -R www-data:www-data ${DEPLOY_DIR}"
                sh "sudo chmod -R 755 ${DEPLOY_DIR}"
                sh 'sudo systemctl reload nginx'
            }
        }
    }

    // --- EMAIL NOTIFICATION BLOCK ---
    post {
        // 🟢 BUILD SUCCESS HONE PAR
        success {
            mail (
                to: env.EMAIL_RECIPIENTS,
                subject: "✅ SUCCESS: Job '${env.JOB_NAME}' [#${env.BUILD_NUMBER}]",
                body: """
--------------------------------------------------
Deployment Successful!
--------------------------------------------------
Angular project successfully build aur Nginx server par deploy ho gaya hai.

Project Name: ${env.JOB_NAME}
Build Number: #${env.BUILD_NUMBER}
Build Link: ${env.BUILD_URL}
""",
                mimeType: 'text/plain'
            )
        }
        
        // 🔴 BUILD FAIL HONE PAR
        failure {
            mail (
                to: env.EMAIL_RECIPIENTS,
                subject: "❌ FAILURE: Job '${env.JOB_NAME}' [#${env.BUILD_NUMBER}]",
                body: """
--------------------------------------------------
Deployment Failed!
--------------------------------------------------
Deployment me koi error aaya hai. Kripya Jenkins log check karein.

Project Name: ${env.JOB_NAME}
Build Number: #${env.BUILD_NUMBER}
Console Logs Link: ${env.BUILD_URL}console
""",
                mimeType: 'text/plain'
            )
        }
    }
}