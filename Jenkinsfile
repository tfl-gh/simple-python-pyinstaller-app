pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    environment {
        TEST_JSON = '1234'
    }
    stages {
        stage('Build') {
            steps {
                env.TEST_JSON = '5678'
                echo "test is ${TEST_JSON}"
                sh 'python3 -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
