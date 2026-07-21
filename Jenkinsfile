pipeline {
    agent any
    
    tools {
        nodejs 'node20'
    }
    
    environment {
        DEPLOY_DIR = '/var/www/myapp'
        PROJECT_NAME = 'project' 
        
        // 📧 Jin-jin logon ko email bhejna hai yahan comma (,) laga kar add karein
        EMAIL_RECIPIENTS = 'afridisalmankhan@gmail.com,000sultankhan@gmail.com'    }

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
            emailext (
                to: "${env.EMAIL_RECIPIENTS}",
                subject: "✅ SUCCESS: Job '${env.JOB_NAME}' [#${env.BUILD_NUMBER}]",
                body: """
                    <div style="font-family: Arial, sans-serif; padding: 15px; border: 2px solid #28a745;">
                        <h2 style="color: #28a745;">🎉 Deployment Successful!</h2>
                        <p>Angular project successfully build aur Nginx server par deploy ho gaya hai.</p>
                        <hr>
                        <p><b>Project Name:</b> ${env.JOB_NAME}</p>
                        <p><b>Build Number:</b> #${env.BUILD_NUMBER}</p>
                        <p><b>Build Link:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                    </div>
                """,
                mimeType: 'text/html'
            )
        }
        
        // 🔴 BUILD FAIL HONE PAR
        failure {
            emailext (
                to: "${env.EMAIL_RECIPIENTS}",
                subject: "❌ FAILURE: Job '${env.JOB_NAME}' [#${env.BUILD_NUMBER}]",
                body: """
                    <div style="font-family: Arial, sans-serif; padding: 15px; border: 2px solid #dc3545;">
                        <h2 style="color: #dc3545;">⚠️ Deployment Failed!</h2>
                        <p>Deployment me koi error aaya hai. Kripya Jenkins log check karein.</p>
                        <hr>
                        <p><b>Project Name:</b> ${env.JOB_NAME}</p>
                        <p><b>Build Number:</b> #${env.BUILD_NUMBER}</p>
                        <p><b>Console Logs Link:</b> <a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a></p>
                    </div>
                """,
                mimeType: 'text/html'
            )
        }
    }
}