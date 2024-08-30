node {
    checkout scm

    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }

    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            junit 'test-reports/results.xml'
        }
    }

    stage('ManualApproval') {
        def userInput = input message: 'Lanjutkan ke tahap Deploy?', parameters: [booleanParam(defaultValue: false, description: 'Klik Yes untuk melanjutkan', name: 'Proceed')]
        if (userInput) {
            echo 'Deploying...'
        } else {
            error 'Approval dibatalkan'
        }
    }

    stage('Deploy') {
        docker.image('python:3.7-alpine').inside('--user root') {
            sh 'apk add --no-cache binutils'
            sh 'pip install --upgrade pip'
            sh 'pip install pyinstaller'
            sh 'pyinstaller --onefile sources/add2vals.py'
            archiveArtifacts 'dist/add2vals'
            sh 'chmod +x dist/add2vals'
            sh 'sleep 60'
            sh './dist/add2vals 50 60'
        }
    }
}