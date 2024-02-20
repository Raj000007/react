pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Your build steps here, e.g., npm install and npm run build for React app
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Deploy to S3') {
            steps {
                script {
                    // Create S3 bucket using CloudFormation
                    def stackName = 'MyReactAppBucket'
                    def bucketName = 'my-react-app-bucket'
                    def templateBody = readFile('path/to/your/cloudformation/template.yaml')

                    def createStack = sh (
                        script: "aws cloudformation create-stack --stack-name ${stackName} --template-body \"${templateBody}\" --parameters ParameterKey=BucketName,ParameterValue=${bucketName}",
                        returnStdout: true
                    )
                    echo createStack

                    // Upload index.html and error.html to S3 bucket
                    sh "aws s3 cp path/to/index.html s3://${bucketName}/index.html"
                    sh "aws s3 cp path/to/error.html s3://${bucketName}/error.html"

                    // Configure S3 bucket for static website hosting
                    sh "aws s3 website s3://${bucketName} --index-document index.html --error-document error.html"
                }
            }
        }
    }
}
