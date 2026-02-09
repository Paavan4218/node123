pipeline {
    agent any

    environment {
        MONGO_URL   = credentials('MONGO_URL')
        JWTSECRETE  = credentials('JWTSECRETE')
        EXPIRES_IN  = credentials('EXPIRES_IN')
        EMAIL       = credentials('EMAIL')
        PASSWORD    = credentials('PASSWORD')
        PORT        = "5000"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/fullstacktraning/nodejs-appln.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t node-backend-app .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                docker rm -f node-backend || true
                docker run -d \
                  -p 5000:5000 \
                  --name node-backend \
                  -e MONGO_URL=$MONGO_URL \
                  -e JWTSECRETE=$JWTSECRETE \
                  -e EXPIRES_IN=$EXPIRES_IN \
                  -e EMAIL=$EMAIL \
                  -e PASSWORD="$PASSWORD" \
                  node-backend-app
                '''
            }
        }
    }
}