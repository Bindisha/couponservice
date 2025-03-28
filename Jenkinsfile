pipeline {
    agent any

 	tools {
        maven 'Maven_Jenkins' // Use the name you provided in the Global Tool Configuration
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
        ECR_REPO_NAME = 'cicd/flightservice'
        IMAGE_TAG = "${env.BRANCH_NAME}-${env.BUILD_ID}"
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
        
/*        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}")
                }
            }
        }*/

        /*stage('Push Docker Image to ECR') {
            steps {
                script {
                    sh """
                    aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }

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