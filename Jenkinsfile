pipeline {
         agent any
         stages {
                 stage('One') {
                 steps {
                     echo 'Hi, lets begin your pipeline!'
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