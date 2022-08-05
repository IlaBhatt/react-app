pipeline {
  agent any
  tools {nodejs "NodeJS"}

  stages {

	stage('Checkout code'){
	 steps {
		git credentialsId: 'f2801aed-98de-4457-ac54-6eaac306ab2f', url: 'https://tools.publicis.sapient.com/bitbucket/scm/pmap/react-app.git', branch : 'feature/registration'
		}
	}
	
	stage('Build project'){
	 steps {
		sh label: '', 
		script: 'npm install'
		}	
	}
	stage('Test project'){
		steps{
			echo 'Testing.................................................................................'
			sh 'npm test'
		}
	}

	stage('Test Coverage project'){
		steps{
			echo 'Testing Coverage........................................................................'
			sh 'npm run test-coverage'
		}
	}

	stage('SonarQube analysis') {
        environment {
            scannerHome = tool 'sonarqube-scanner'
        }
        steps {
            withSonarQubeEnv('sonarqube-scanner') {
                sh '''
                ${scannerHome}/bin/sonar-scanner \
				-D sonar.login=admin \
				-D sonar.password=sonarqube \
                -D sonar.projectKey=PMAP \
                -D sonar.projectName=React-App \
                -D sonar.projectVersion=0.1.0 \
				-D sonar.javascript.file.suffixes=.js,.jsx \
                -D sonar.sources=./src \
				-D sonar.tests=./src \
                -D sonar.test.inclusions=**/__tests__/*.test.js \
                -D sonar.exclusions=./node_modules \
				-D sonar.javascript.lcov.reportPaths=./coverage/lcov.info \
				-D sonar.testExecutionReportPaths=test-report.xml
                '''
            }
        }
    }
   }

 }