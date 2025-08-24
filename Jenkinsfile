pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'jdk17'
    }

    stages {

       stage('SCM GitHub') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/onurglr/devops-02-pipeline']])
            }
        }

        stage('Build Maven') {
            steps {
            //    sh 'mvn clean install'
                bat 'mvn clean install'
            }
        }


        stage('Test Maven') {
            steps {
            //    sh 'mvn test'
                bat 'mvn test'
            }
        }


        stage('Docker Image') {
            steps {
            //    sh 'docker build  -t mimaraslan/devops-application:latest   .'
                bat 'docker build  -t onurguler18/devops-application:latest   .'
            }
        }


        stage('Docker Image To DockerHub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {

                        if (isUnix()) {
                             sh 'docker login -u onurguler18 -p %dockerhub%'
                             sh 'docker push onurguler18/devops-application:latest'
                          } else {
                             bat 'docker login -u onurguler18 -p %dockerhub%'
                             bat 'docker push onurguler18/devops-application:latest'
                         }
                    }
                }
            }
        }
    



        stage('Deploy Kubernetes') {
            steps {
            script {
                    kubernetesDeploy (configs: 'deployment-service.yaml', kubeconfigId: 'kubernetes')
                }
            }
        }


        stage('Docker Image to Clean') {
            steps {
              
                   //  sh 'docker image prune -f'
                     bat 'docker image prune -f'
               
            }
        }



    }

}


