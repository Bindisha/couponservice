pipeline {
    agent any


 	tools {
		jdk 'JDK17'
        maven 'Maven_Jenkins' // Use the name you provided in the Global Tool Configuration
        dockerTool 'Docker_Jenkins'      
          }
    
   


    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
    }

    triggers {
        pollSCM('H/5 * * * *')
    }

    environment {
        AWS_ACCOUNT_ID = '773120352783'
        AWS_REGION = 'ap-south-1'
        ECR_REPO_NAME = 'cicd/couponservice'
        IMAGE_TAG = "${env.BRANCH_NAME}-${env.BUILD_ID}"
        AUTH_PASSWORD = 'eyJwYXlsb2FkIjoiMW9PMGxmRU51OWkxU0VWMjF0ZjJZNnp6NXJPa1J4UlVCT3JEZ3VDQlFXVzlKZnlFWUhZODJMSnVvZHZMc1J0RHRsb0NncTlqalpLYmhpM2J1bThIQ3JwMHZMVEQ5VFNvbURtWldNVU8rQ2Y2cmpDdlVkMW9iZDlkSTFRMWtFZ1puZ2swNEpnQmgxN2o5QmZzb08rSnR4MkFHc0NuWnpSSkl6L1FQYlRaTWpZeENQNVp4RFp0TEllNEx4elQ0NnRQZFUzNkt5YmFoZHVnRisycytsR3FqOHdUeEZpbGRRRHFlQ3NPMHF4Tkp2Uk1WbUo4WFAzckIrYUhjOVdZaVJidkpaRCtzd0VvSFlQSnV3c1dEMVhrZ3ZMdGlvb2lzVnpuSWJzZWU0ZkRBRzdWWWFnWmVlS0laUnY1NVBMRWlENGZiMDJMa3hnR1M1NHpkNTN4Ym5mOWlFRnJTcnRuYVBGSFZEZ3NxSGJmUFVENmRlNTVRY2ZVT2p6ME1xcnNwa0dNdm1FeU1pdnRjb09GZEhid2lxaE1TVTZEVHVGaTlZTWF0UWgyMGVvcnZSaVVjK0llMmFOY2hLUm9Fb0RJZmRNK2ZLZ1oyckVlODEvSGxvUDFrclF4YlpTTEQ2WHlJQ1Z4YUlQMEpaT0NmZWZyaS9wYllIV3lRUHNYL3BIbDZBUXdXM3JGK0JzV3ZaRCtuQnZMV3RTbEkrUGg2YWRpdEZHZlk0T1hkV2ZkREFtUW5JUDZTUmEzdUMzV2lPKzZhdVZXallQaHZFWCtlclVMckxvK3NiNzZlQXJMaENXeVZQUDNYZjg4QjJwWWdTRHR2Y2dUYm1MdENaRGNqc2oreCtJbDFBS0hSTzdEMTdNT2N1bWJhWnQ1OEZYL3FVNDFOUzMrKzFQalhlVlF3OWo4NFlyQUNBNHByNUpNdldjUUtLdVZaWUJFalhnVUtQWTBTcGVpZU9SZzdrVU44eUFyR21IZzY4a2ptRGRCWDRGUlNLQjRFWSs5TFRNdkhTTDFUbGZJeE0xRnlHVExvbGlPRlBiRFQwL2Z2TlhTVkxCWVU5TDJwTXFuVURkWTdhQ200V1JGZ1NBcTRmQWFLQzVYZk81c0EyM3VWMm5VTXRpUmQ5djFiRFhWMXQ0U1haVkM4MTBPWk91SFVnNEd1U2JWaWFONTFTYWZyVUs2SmJLckltUERVNDZPRUhSNHJySzFMNDZJMjl3eXpHTVpHTlRpNEFQaXovV0FScGJEcE0xVUVxejMvdkptVGY1TUNWUFl2L1hOUmQ1QmJUcHJwOGtpU1k1NGRUcWtkbEY4Tmkya0Z0WHlFb1R4RlNXaGFnK3k1VG9ZOFdsNGUrTnFtaERIWUtMcFJnPT0iLCJkYXRha2V5IjoiQVFJQkFIaUhXYVlUblJVV0NibnorN0x2TUcrQVB2VEh6SGxCVVE5RnFFbVYyNkJkd3dFYTNkd3ZWbUFRTFJpOXJBM0Q2Rzc2QUFBQWZqQjhCZ2txaGtpRzl3MEJCd2FnYnpCdEFnRUFNR2dHQ1NxR1NJYjNEUUVIQVRBZUJnbGdoa2dCWlFNRUFTNHdFUVFNQURERi9RbjU1Z2FoN0NPcEFnRVFnRHVWR1FPbEZsWWJFYWJYZUtMQVFXSVJkVzMxL1hhYzFCS3JkY1I4L2thckl1RjJObm8rczBIdjZ0bEcyR2hEN2dpQ1JodDYwbWxOUnI3ZWp3PT0iLCJ2ZXJzaW9uIjoiMiIsInR5cGUiOiJEQVRBX0tFWSIsImV4cGlyYXRpb24iOjE3NDMyODIxMzV9'
        EC2_USER = 'ec2-user' // Change if using Ubuntu (e.g., 'ubuntu')
        EC2_HOST = 'ec2-65-0-91-202.ap-south-1.compute.amazonaws.com'
        SSH_KEY = '/var/tmp/bindisha0101-db-jar-key1.pem'
        PORT_NO = '8081'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        checkout scm
                    } else {
                        checkout([$class: 'GitSCM', branches: [[name: "*/${params.BRANCH_NAME}"]], userRemoteConfigs: [[url: 'https://github.com/Bindisha/couponservice.git']]])
                    }
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Code Coverage') {
            steps {
                sh 'mvn jacoco:report'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool 'SonarQubeScanner'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=coupon_service_key \
                    -Dsonar.projectName=coupon_service \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=src/main/java \
                    -Dsonar.java.binaries=target/classes \
                    -Dsonar.junit.reportPaths=target/surefire-reports \
                    -Dsonar.jacoco.reportPaths=target/jacoco.exec
                    """
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
            def dockerImage = sh(script: "sudo docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG} .", returnStdout: true).trim()
        }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
					
					def token = sh(script: "aws ecr get-login-password --region ${AWS_REGION}", returnStdout: true).trim()
                    sh """
                    sudo docker login --username AWS -p ${token} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    sudo docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    sh """
                    ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ${EC2_USER}@${EC2_HOST} << 'EOF'
                        echo "Logging into AWS ECR..."
                        aws ecr get-login-password --region ${AWS_REGION} | sudo docker login --username AWS --password ${AUTH_PASSWORD} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com

                        echo "Pulling latest Docker image..."
                        sudo docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}

                        echo "Stopping existing container (if any)..."
                        sudo docker stop couponservice || true
                        sudo docker rm couponservice || true

                        echo "Running new container..."
                        sudo docker run -d --name couponservice -p ${PORT_NO}:8080 --restart always \
                        ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                        
                        echo "Deployment Successful!"
                    EOF
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
            jacoco execPattern: '**/target/jacoco.exec', classPattern: '**/classes', sourcePattern: '**/src/main/java', exclusionPattern: '**/src/test*'
        }
        failure {
            echo "Pipeline execution failed!"
        }
    }
}
