pipeline {
agent any


environment {
AWS_REGION="ap-south-1"
S3_BUCKET= "assesment-s3-210795"
CLOUDFRONT_DISTRIBUTION_ID= "E18E61W9ACOZIM"
}
stages{
stage ('1.Checkout'){
steps{
git branch: 'main', url: 'https://github.com/akshayshetty709/abc-frontend.git'
}
}
stage ('2.install dependencies and build'){
steps{
sh """
npm ci
npm run build 
"""
}
}
stage ('3. upload files to s3 and invalidate cloundfront'){
steps{
withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWS-Cred', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
sh """
aws s3 sync dist/  s3://$S3_BUCKET
aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_DISTRUBUTION_ID --paths "/*"
"""
}
}
}
}
}

