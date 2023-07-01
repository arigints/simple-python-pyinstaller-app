node {
    checkout scm
    stage('Build'){
        docker.image('python:2-alpine').inside{
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    try {
        stage('Test'){
            docker.image('qnib/pytest').inside{
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        }
    } finally {
        junit 'test-reports/results.xml'
    }

    try {
        stage('Deliver'){
            docker.image('cdrx/pyinstaller-linux:python2').inside{
                sh 'pyinstaller -F sources/add2vals.py'
            }
        }
        echo 'This will run only if successful'
    } catch (exc) {
        echo 'This will run only if failed'
    }
}