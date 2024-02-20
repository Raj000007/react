pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1' // Replace 'your-region' with your desired AWS region
    }

    stages {
        stage('Deploy to S3') {
            steps {
        

                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    script {
                        // Fetch the content of template.yaml from the local repository
                        def templateBody = readFile('templatate.yaml')

                        // Create S3 bucket using CloudFormation with the specified region and credentials
                        def createStack = sh (
                            script: "aws cloudformation create-stack --stack-name MyReactAppBucket --template-body \"${templateBody}\" --parameters  --region ${AWS_DEFAULT_REGION}",
                            returnStdout: true
                        )
                        echo createStack
                       

                        // Wait for the bucket creation to complete
                        waitForBucketCreation()
                        
                        // Upload index.html and error.html to S3 bucket
                        sh "aws s3 cp index.html s3://react-app-bucket-unique/index.html --region ${AWS_DEFAULT_REGION}"
                        sh "aws s3 cp error.html s3://react-app-bucket-unique/error.html --region ${AWS_DEFAULT_REGION}"

                        // Configure S3 bucket for static website hosting
                        sh "aws s3 website s3://react-app-bucket-unique --index-document index.html --error-document error.html --region ${AWS_DEFAULT_REGION}"
                    }
                }
            }
        }
    }
}
