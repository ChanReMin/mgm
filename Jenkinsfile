#!/usr/bin/env groovy

import groovy.transform.Field

@Field
String DOCKER_USER_REF = 'dockerhub_id'
@Field
String SSH_ID_REF = 'ec2_id'
@Field
String ec2ipv4 = '13.229.99.110'

pipeline {
    agent any

    tools {
        dockerTool 'docker'
    }

    stages {
        stage('build and test') {
            steps {
                sh '''
                cd app
                docker build -t longtch/todo-nodejs:1.0.0 .
                '''
            }
        }
        stage('Docker login and push docker image') {
            steps {
                withBuildConfiguration {
                    sh '''
                    docker login -u ${username} -p ${password}
                    docker push longtch/todo-nodejs:1.0.0
                    '''
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    withBuildConfiguration {
                        sshagent(credentials: [SSH_ID_REF]) {
                            sh 'echo "Deploying docker image to EC2"'
                            def dockerCmd = '''
                            sudo yum update -y
                            sudo yum install docker -y
                            sudo systemctl start docker
                            sudo systemctl enable docker
                            sudo usermod -aG docker ${whoami}
                            sudo chmod -R 777 /var/run/docker.sock
                            docker run -p 80:8000 -d longtch/todo-nodejs:1.0.0
                            '''
                            sh "ssh -o StrictHostKeyChecking=no ec2-user@${ec2ipv4} '${dockerCmd}'"
                        }
                    }
                }
            }
        }
    }
}

void withBuildConfiguration(Closure body) {
    withCredentials([usernamePassword(credentialsId: DOCKER_USER_REF, usernameVariable: 'username', passwordVariable: 'password')]) {
        body()
    }
}
