pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1' // Replace 'your-region' with your desired AWS region
    }

    stages {
        

        stage('Deploy to S3') {
            steps {
                script {
                    // Create S3 bucket using CloudFormation
                    def stackName = 'MyReactAppBucket'

                    // Fetch the content of template.yaml from GitHub
                    def templateBody = sh(
                        script: 'curl -sSL https://raw.githubusercontent.com/Raj000007/react/main/template.yaml',
                        returnStdout: true
                    ).trim()

                    // Create CloudFormation stack with the specified region
                    def createStack = sh (
                        script: "aws cloudformation create-stack --stack-name ${stackName} --template-body \"${templateBody}\" --parameters ParameterKey=BucketName,ParameterValue=${stackName} --region ${AWS_DEFAULT_REGION}",
                        returnStdout: true
                    )
                    echo createStack

                    // Upload index.html and error.html to S3 bucket
                    sh "aws s3 cp path/to/index.html s3://${stackName}/index.html --region ${AWS_DEFAULT_REGION}"
                    sh "aws s3 cp path/to/error.html s3://${stackName}/error.html --region ${AWS_DEFAULT_REGION}"

                    // Configure S3 bucket for static website hosting
                    sh "aws s3 website s3://${stackName} --index-document index.html --error-document error.html --region ${AWS_DEFAULT_REGION}"
                }
            }
        }
    }
}
