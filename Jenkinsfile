pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        docker {
          image 'python:2-alpine'
        }

      }
      steps {
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
      }
    }
    stage('Test') {
      agent {
        docker {
          image 'qnib/pytest'
        }
      }
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
      agent any
      steps {
        sh 'docker run --rm -v $(pwd -P):/src cdrx/pyinstaller-linux:python2 "pyinstaller --onefile sources/add2vals.py" '
      }
      /*
      post {
        success {
          archiveArtifacts 'dist/add2vals'
        }
      }
      */
    }
  }
}
