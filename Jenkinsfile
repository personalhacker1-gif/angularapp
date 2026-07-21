pipeline {
    agent any
    
    tools {
        nodejs 'node20'
    }
    
    environment {
        // Nginx jahan se website serve karega
        DEPLOY_DIR = '/var/www/myapp'
        // Apne angular.json me jo 'projectName' hai yahan likhein (jaise 'my-angular-app')
        PROJECT_NAME = 'your-angular-app-name' 
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
                
                // 1. Directory agar bani nahi hai toh banao
                sh "sudo mkdir -p ${DEPLOY_DIR}"
                
                // 2. Old build ko saaf karo taaki fresh deployment ho
                sh "sudo rm -rf ${DEPLOY_DIR}/*"
                
                // 3. Angular ka build files copy karo 
                // Note: Angular 17/18+ me path 'dist/PROJECT_NAME/browser/*' hota hai.
                // Purane Angular versions me path 'dist/PROJECT_NAME/*' hota hai.
                sh "sudo cp -r dist/${PROJECT_NAME}/browser/* ${DEPLOY_DIR}/ || sudo cp -r dist/${PROJECT_NAME}/* ${DEPLOY_DIR}/"
                
                // 4. Nginx readable permissions set karo
                sh "sudo chown -R www-data:www-data ${DEPLOY_DIR}"
                sh "sudo chmod -R 755 ${DEPLOY_DIR}"
                
                // 5. Nginx reload karo
                sh 'sudo systemctl reload nginx'
            }
        }
    }
}