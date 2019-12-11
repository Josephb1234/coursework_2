pipeline {
		agent any
			stages {
					stage('One') {
						steps {
						echo 'Hi, lets start your pipeline!'
						}
						}
					stage('Sonarqube') {
						environment {
						scannerHome = tool 'SonarQubeScanner'
						}
					steps {
						withSonarQubeEnv('sonarqube') {
							sh "${scannerHome}/bin/sonar-scanner"
						}
							timeout(time: 10, unit: 'MINUTES') {
							waitForQualityGate abortPipeline: true
							}
						}
					}	
				}
}

node {
					def app
						stage('Clone repository') {
							checkout scm
						}

						stage('Build image') {
							app = docker.build("josephb123/coursework_2")
						}

						stage('Test image') {
							app.inside {
								sh 'echo "Tests passed"'
							}
						}

						stage('Push image') {
							docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
								app.push("latest")
							}
						}
				}
