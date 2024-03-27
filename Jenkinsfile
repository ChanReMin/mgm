#!/usr/bin/env groovy

import groovy.transform.Field

@Field
String DOCKER_USER_REF = 'dockerhub_id'
@Field
String SSH_ID_REF = '1'

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
                    echo "${password}"
                    '''
                }
            }
        }
        // stage('deploy') {
        //     steps {
        //         withBuildConfiguration {
        //             sshagent(credentials: [SSH_ID_REF]) {
        //                 sh '''
        //                 echo 'Deploying docker image to EC2'
        //                 def dockerCmd = "docker run -p 8080:8080 -d longtch/todo-nodejs:1.0.0"
        //                 sshagent(['ec2-server']) {
        //                 sh "ssh -o StrictHostKeyChecking=no ec2-user@54.175.82.44 ${dockerCmd}"
        //                 '''
        //             }
        //         }
        //     }
        // }
    }
}

void withBuildConfiguration(Closure body) {
    withCredentials([usernamePassword(credentialsId: DOCKER_USER_REF, usernameVariable: 'username', passwordVariable: 'password')]) {
        body()
    }
}
