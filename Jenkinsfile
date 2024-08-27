node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python --version'
            sh 'echo "Hello"'
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
}
