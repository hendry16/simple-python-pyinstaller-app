node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python --version'
            sh 'echo "Hello"'
        }
    }
}
