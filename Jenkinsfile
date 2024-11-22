pipeline {
    agent any
    stages {
        stage('Cleanup') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Git Repo') {
            steps {
                checkout scm
            }
        }
        stage('Clone from repository') {
            steps {
                git url: 'https://github.com/joselin111/2244_ica2.git', branch: 'develop'
            }
        }

        stage('Build and run docker image') {
            steps {
                sh 'sudo docker build -t joselin721/exam2:latest .'
                sh 'sudo docker run -d -p 8081:80 joselin721/exam2:latest'
            } 
        }



        stage('testing') {
            steps {
                sh 'curl -I 192.168.219.163:8081'
            }
        }

        stage('Build and Push') {
            steps {
                echo 'Building..'
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-auth', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            sudo docker login -u ${USERNAME} -p ${PASSWORD}
                            sudo docker push joselin721/exam2:latest
                        '''
                        sh "sudo docker tag joselin721/exam2:latest joselin721/exam2:develop-${env.BUILD_ID}" 
                        sh "sudo docker push joselin721/exam2:develop-${env.BUILD_ID}"
                    }
            }
        }

    
    }
}