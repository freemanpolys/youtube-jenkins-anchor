pipeline {
    agent any
    environment {
        DOCKER_HOST = 'tcp://192.168.50.1:2375'
        DOCKER_REGISTRY = '192.168.50.1:5000'
    }
    stages {
        stage('build') {
            steps {
                script {
                    docker.withTool("docker") {
                        docker.withRegistry("${DOCKER_REGISTRY}") {
                            def image = docker.build('freemanpolys/jenkins-anchore:latest')
                            image.push()
                        }
                    }
                }
            }
        }
        stage('analyze') {
            steps {
                sh 'echo "docker.io/freemanpolys/freemanpolys/jenkins-anchore:lates $PWD/Dockerfile" > anchore_images'
                anchore name: 'anchore_images'
            }
        }
    }
}