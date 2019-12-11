pipeline {
		agent any
			stages {
				stage('One') {
					steps {
					echo 'Hi, lets start your pipeline!'
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

node {
					def app
						stage('Clone repository') {
							checkout scm
						}

						stage('Build image') {
							app = docker.build("josephb1234/coursework_2")
						}

						stage('Test image') {
							app.inside {
								sh 'echo "Tests passed"'
							}
						}

						stage('Push image') {
							docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
								app.push("${env.BUILD_NUMBER}")
								app.push("latest")
							}
						}
				}
