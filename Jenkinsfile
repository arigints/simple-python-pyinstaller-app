node {
    stage('Build'){
        docker.image('python:2-alpine'){
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    try {
        stage('Test'){
            docker.image('qnib/pytest'){
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        }
    } finally {
        junit 'test-reports/results.xml'
    }
    try {
        stage('Deliver'){
            docker.image('cdrx/pyinstaller-linux:python2'){
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
        }
        echo 'This will run only if successful'
    }
}