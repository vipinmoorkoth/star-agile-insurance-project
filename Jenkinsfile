pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')  // Jenkins credential ID for Docker Hub
        IMAGE_NAME = 'vipinms90/finance-me'
        IMAGE_TAG = '1.0'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout your Git repo (adjust URL and branch as needed)
                git branch: 'master', url: 'https://github.com/vipinmoorkoth/star-agile-insurance-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build docker image with tag
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    // Login to Docker Hub with credentials stored in Jenkins
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push image to Docker Hub repo
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Deploy App Node') {
            steps {
                script {
                    // Run Ansible playbook for app node
                    sh 'ansible-playbook -i host.ini app-playbook.yaml'
                }
            }
        }

        stage('Deploy Monitoring Node') {
            steps {
                script {
                    // Run Ansible playbook for monitoring node
                    sh 'ansible-playbook -i host.ini monitoring-playbook.yaml'
                }
            }
        }
    }

    post {
        always {
            script {
                // Logout of Docker Hub after pipeline finishes
                sh 'docker logout'
            }
        }
    }
}
