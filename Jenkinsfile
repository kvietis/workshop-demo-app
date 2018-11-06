pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''# Create python virtual Environment
pip3 install virtualenv --user
python3 -m venv repo

. repo/bin/activate

# Install python requirements
pip install -r requirements.txt'''
      }
    }
    stage('test') {
      steps {
        sh '''# Activate python virtual environment
. repo/bin/activate
#Run pytest and save coverage report
pytest --cov-report xml --cov-report term --cov ./lib/'''
        cobertura(coberturaReportFile: 'coverage.xml', failNoReports: true, failUnhealthy: true, failUnstable: true, lineCoverageTargets: '90,50,80')
      }
    }
    stage('deploy') {
      when {
        branch 'master'
      }
      steps {
        input 'is it ready?'
        sh '''cd deployment/
sh provision.sh'''
      }
    }
  }
}