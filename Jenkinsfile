pipeline {
    agent any

 	tools {
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
        AUTH_PASSWORD = 'eyJwYXlsb2FkIjoiY0MwZmRld3F3RTQyZnF3eTI2YmtxUmNxVVBoU3VibTBQN0szSEZGNEE0b1EzRmJUYnJvdll5aHM5U3FuQmsveUd0YnBjMmE1TldKUWQ2SU93MlFhQnJSTzNrU29wQW9LSlU3L1Y2TDQ0Zk01V1kzQ2VNcDh0TVRqQ1BCZkxaYnVPdjFoZk1iWm0xZG1OSU51MTdJZXBqNnVxMko1ZDhtZ215YWlWU0UvaW1zYUxQa2YzOUdzOHZSWXJUSCtoS3YwTUc3blhzSEswVEc5VWlsd0JGS2lzczZsQVBudFFWUndpWHIvTTdIdlZ3MDA5b1lacWw3T2diZTk2NE0xWTEvNXhNVW10dmpBbkp5R1Z1LzJjSVNBZ2RwT0xRNUVib2FsZTZrY0I0enc4enFnMXpCSkttUkgwTElZMzgxdGxWNm82dW9MNWo3bzF2Si9tUEx3cmpScWplM3czWlZpUlRpc1dhSzlrbDY1U1FPRnA2eEFsS2xBUkIvcTV0ZjJkSC9US0EvTWl3K2VUanMwVnA5bnBnLzVrU1RuMXFwQ0lIemRSMVVzc2gvQVV5b3JZM2p0U01HS2Jma2g3UUhrSXdhL05NOUk5RWZNckhqVHNuQ1l5NHI1NHQ1VlJsOVBQbVFFazZpeU5OcWFoOUZtczFMMVZiNDF0WVA0UFBYM2VaZUVGOHJuRnZYWlJud1JobU85RTJ5ZWx0eGxuMkVmTkQxQjI2c3QyRkRqTlFGOFhtcFlQY05uQUwyRE1SWDNtQkFPT1puckVJeGN0cFpIYUdlZGlNdEJCc1EwbFZnajJIZXYxVzZzYUlRRGRMYmlSK0NDTnhOOFpTb2psL2VMd1FwdTVGWnQzUzNIN0piOHg5ZEw2MVlCMURZZ1N6MG1JVktoNEZicWtzNENlMlAvU2NNa3RvbE5XOFQ0aDUzWXdKTTVqTURXSk04NEcrNWg0eXZqYVRHWmxubVR2OWxldFdwK01IQkFTYzNyWENNcFI3blVwV2hJMU5EK29kcWVkT2RlNHliYnNWS3lUYlplN2ZCSW1HQ2NVclNtNzNoenl4ZEphc0hZS0JieWQyTzZCYVhQM0dGQ2JoTHRsMm8xVkQvYStoWitVOGZOWlVHTWhYMThERkhuL3RpSUVaaFBMelhSSlBrZFFiNWQraWM3RDNXSHJPd1Bia3NoR1BTUjZUSVZXQkpncFZBR3I5RFBpbGd0bEgwaGt5bW5VY2ZOVXNuYysyR3NOYmt2WThSU3UzdCsrOUFJTThJMysza2czYlFQajM4RmhJd1dPR2pOSjFTbkhvTnhKcUl0S1NYL2wvTE0yeU13cFZYcGpNdFhMdlZPbXdvNXdDUzU3ejh0Q1hKdm1nPT0iLCJkYXRha2V5IjoiQVFJQkFIaUhXYVlUblJVV0NibnorN0x2TUcrQVB2VEh6SGxCVVE5RnFFbVYyNkJkd3dFSGRvWkJwYzRxMVE2aG5WL0NUd1o2QUFBQWZqQjhCZ2txaGtpRzl3MEJCd2FnYnpCdEFnRUFNR2dHQ1NxR1NJYjNEUUVIQVRBZUJnbGdoa2dCWlFNRUFTNHdFUVFNd1oreVMrRTFjRENxQ2QvYUFnRVFnRHRPRkRqUVBHNmpYL0hKc2U0N1hTRzhIRGRnQXBnVFBIbWlURjMrWStXMERHeVRhbkdRaG9RZHNLeHRFSmEwRUNjZUQ5U3ZhcjZCYVJvNFhBPT0iLCJ2ZXJzaW9uIjoiMiIsInR5cGUiOiJEQVRBX0tFWSIsImV4cGlyYXRpb24iOjE3NDMyMzMwMDR9'
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
                    -Dsonar.tests=src/test/java \
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
                    sh """
                    aws ecr get-login-password --region ${AWS_REGION} | sudo docker login --username AWS --password ${AUTH_PASSWORD} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    sudo docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }
/*
        stage('Deploy to AWS') {
            steps {
                script {
                    sh 'aws ecs update-service --cluster your-cluster-name --service your-service-name --force-new-deployment'
                }
            }
        }*/
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