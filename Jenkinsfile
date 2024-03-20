pipeline{
    agent any
    environment {
        PROJECT_ID = "student-00456"
        CLUSTER_NAME = "public-cluster"
        LOCATION = "us-central1-a"
        CREDENTIALS_ID = 'Credentials'
    }
    stages{
        stage('Checkout from GitHub') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/LucaTatoli/Todo-Application']])
            }
        }
        stage('Building Docker file'){
            steps {
                script{
                    sh "docker build -t student00456/todo_app:latest ."
                }
            }
        }
        stage('Pushing Docker file to DockerHub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'Docker_Password', variable: 'docker')]){
                    sh 'docker login -u student00456 -p latest'
                    }
                    sh "docker push student00456/todo_app:latest"
                }
            }
        }
        stage('Deploy to Kubernetes'){
            steps{
                sh "sed -i 's/tagversion/latest/g' Deployment.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'Deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])   
                }
            }
        }
    }
