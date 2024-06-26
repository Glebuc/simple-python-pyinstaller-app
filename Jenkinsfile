pipeline {
    agent any 

    environment {
        PYTHONIOENCODING = 'utf-8'
    }

    stages {
        stage('Build') { 
            steps {
                bat 'python -m py_compile ./sources/add2vals.py ./sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
        stage('Test') {
            steps {
                bat 'python -m pytest --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                bat "python -m pyinstaller --onefile sources/add2vals.py"
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}