pipeline {
    agent any 
    stages {
        stage('Set Up Python Environment') {
            steps {
                // Install Python and pip (if not already available on your Jenkins agent)
                bat 'python -m venv venv'
                bat 'venv\\Scripts\\python -m pip install --upgrade pip'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Activate the virtual environment and install pytest
                bat 'venv\\Scripts\\pip install pytest'
            }
        }
        stage('Build') { 
            steps {
                bat 'python -m py_compile sources/add2vals.py sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
        stage('Test') {
            steps {
                bat 'venv\\Scripts\\python -m pytest --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
    }
}