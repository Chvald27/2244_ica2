pipeline {
    agent any
    stages {
        stage('Cleanup'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout the code') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                //echo "This is Build Docker image stage 3"
		//echo "This is a Test 3"
                sh 'docker build -t 2244_ica2:latest .'
                sh "docker tag 2244_ica2:latest 2244_ica2:develop-${env.BUILD_ID}"
                sh 'docker images'
                sh 'docker run -d -p 8081:80 2244_ica2:latest'
                sh 'docker ps'
            }
        }
        stage('Testing Website Accessibility'){
            steps {
                sh 'curl -I localhost:8081'
            }
        }
        stage('Tagging and Push'){
            steps {
                sh 'docker tag 2244_ica2:latest secarl/2244_ica2:latest'
                echo 'Building..'
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-auth', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            docker login -u ${USERNAME} -p ${PASSWORD}
                            docker push secarl/2244_ica2:latest
                        '''
                        //sh "sudo docker push sanjeebnepal/devops_exam2:develop-${env.BUILD_ID}" Test
                    }

            }
        }

    }

}
