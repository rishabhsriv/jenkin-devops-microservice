pipeline { 
	agent any
	// agent {
	// 	 docker{
	// 		image 'maven:3.6.3'
	// 		}
	// }
	// agent{
	// 	docker{
	// 		image 'node:15.7'
	// 	}
    //   }
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages{
		stage('Checkout'){
			steps {
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "Path - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUID_URL - $env.BUILD_URL"
			}
		}
		stage('Compile'){
			steps {
				sh "mvn compile"
			}
		}
		stage('Test'){
			steps{
				sh "mvn test"
			}
		}
		stage('Integration Test'){
			steps{
				sh "mvn failsafe:integration-test failsafe:verify"
			}
		}
		stage('Package'){
			steps{
				sh "mvn package"
			}
		}
		stage('Build Docker Image'){
			//"docker build -t rishabhsriv13/currency-exchange-devops:$env.BUILD_TAG"
			steps{
				script{
				dockerImage = docker.build("rishabhsriv13/currency-exchange-devops:${env.BUILD_TAG}")
				}
			}
		}
		stage('Push Docker Image'){
			steps{
				script{
					docker.withRegistry('', 'dockerhub'){
						dockerImage.push();
						dockerImage.push('latest');
					}
				}
			}
		}
	}
	post {
		always{
			echo 'I run always'
		}
		success{
			echo 'I run when you are successful'
		}
		failure{
			echo 'I run when you fail'
		}
	}
	
}
//Declarative
