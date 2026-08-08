pipeline {
    agent any

    environment {
        BUCKET_NAME = "frontendd-deploy1000669"
        DISTRIBUTION_ID = "E1W4HWTC0AL6TO"
    }

    stages {
       stage('checkout') {
        steps {
            checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GIT-CRED', url: 'https://github.com/anuragpm1992-sketch/abc-frontend.git']])
        }
    }
        stage('install dependencies') {
            steps {
            sh 'npm install'
            sh 'npm run build'
        }
    }
        stage('deployment in S3 bucket') {
            steps {
            sh 'aws s3 sync build/ s3://$BUCKET_NAME --delete'
        }
    }
        stage('cloudfront invalidaation') {
            steps {
            sh "aws cloudfront create-invalidation --distribution-id $DISTRIBUTION_ID --paths '/*'"

        }
    }
}
}
post {
    success {
        echo 'deployment success'
    }
    failure {
        echo 'deplyment failed'
    }
}
