pipeline {
         agent any
         stages {
                 stage('One') {
                 steps {
                     echo 'Hi, lets start your pipeline!'
                 }
                 }
				node {
					def app
						stage('Clone repository') {
							/* Let's make sure we have the repository cloned to our workspace */

							checkout scm
						}

						stage('Build image') {
							/* This builds the actual image; synonymous to
							 * docker build on the command line */

							app = docker.build("getintodevops/hellonode")
						}

						stage('Test image') {
							/* Ideally, we would run a test framework against our image.
							 * For this example, we're using a Volkswagen-type approach ;-) */

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
