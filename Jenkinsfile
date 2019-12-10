pipeline {
         agent any
         stages {
                 stage('One') {
                 steps {
                     echo 'Hi, this is Zulaikha from edureka'
                 }
                 }
                 stage('Two') {
                 steps {
                    input('Do you want to proceed?')
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
                 stage('Three') {
                 when {
                       not {
                            branch "master"
                       }
                 }
                 steps {
                       echo "Hello"
                 }
                 }
                 stage('Four') {
                 parallel { 
                            stage('Unit Test') {
                           steps {
                                echo "Running the unit test..."
                           }
                           }
                            stage('Integration test') {
                              agent {
                                    docker {
                                            reuseNode true
                                            image 'ubuntu'
                                           }
                                    }
                              steps {
                                echo "Running the integration test..."
                              }
                           }
                           }
                           }
              }
}