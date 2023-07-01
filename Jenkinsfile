node {
    stage('Build'){
        docker.image('python:2-alpine'){
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    try {
        stage('Test'){
            docker.image('qnib/pytest'){
                checkout scm
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        }
    } finally {
        junit 'test-reports/results.xml'
    }
    try {
        stage('Deliver'){
            docker.image('cdrx/pyinstaller-linux:python2'){
                checkout scm
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
        }
        echo 'This will run only if successful'
    } catch (err){
        echo err.getMessage()
        echo 'This will run only if failed'
    }
}