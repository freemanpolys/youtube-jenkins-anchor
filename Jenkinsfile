pipeline {
    agent any
    environment {
        DOCKER_HOST = 'tcp://192.168.50.1:2375'
    }
    stages {
        stage('build') {
            steps {
                script {
                    docker.withTool("docker") {
                        withCredentials([usernamePassword(credentialsId : "dockerhub",passwordVariable : 'DOCKER_PASSWORD' ,usernameVariable : 'DOCKER_USERNAME')]) {
                            sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                            def image = docker.build("freemanpolys/jenkins-anchore:${BUILD_ID}")
                            image.push()
                        }    
                    }
                }
            }
        }
        stage('analyze') {
            steps {
                writeFile file: "anchore_images", text: "docker.io/freemanpolys/jenkins-anchore:${BUILD_ID}"
                anchore name: 'anchore_images',bailOnFail: false, bailOnPluginFail: false
            }
        }
    }
}