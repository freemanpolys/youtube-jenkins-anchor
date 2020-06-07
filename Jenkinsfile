pipeline {
    agent any
    environment {
        IMAGE = 'registry.labs:5000/scratch:latest'
    }
    stages {
        stage('analyze') {
            steps {
                writeFile file: "anchore_images", text: "${IMAGE}"
                writeFile file: "Dockerfile", text: "FROM ${IMAGE}"
                anchore name: 'anchore_images',bailOnFail: true, bailOnPluginFail: true
            }
        }
    }
}
