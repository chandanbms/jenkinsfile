pipeline {
	agent any
	stages {
		stage('Checkout') {
			steps {
					git 'https://github.com/chandanbms/repo'
				}
		}
		
		stage('Compile') {
		    steps {
		        dir('') {
		             sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/myMaven/bin/mvn compile'
		        }
		    }
		}
		
		stage('Review') {
		    steps {
		        dir('') {
		             sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/myMaven/bin/mvn -P metrics pmd:pmd'
		        }
		    }
		}
		
		stage('Test') {
		    steps {
		        dir('') {
		             sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/myMaven/bin/mvn test'
		        }
		    }
		}
		
		
		stage('Build') {
		    steps {
						dir('') {
						sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/myMaven/bin/mvn -B -V -U -e clean package'
						}
					}
		}
		
		stage('Dockerfile') {
		    steps {
					       dir('')	{
                        sh 'rm -f /var/lib/jenkins/workspace/CI_CD_pipeline/addressbook.war'
						sh 'cp /var/lib/jenkins/workspace/CI_CD_pipeline/target/addressbook.war /var/lib/jenkins/workspace/CI_CD_pipeline'
						sh 'docker build -f /root/chandan/manifest/dockerfile_addr . -t chandanbms/addressbook:latest'
						}
					}
		}
		
		stage('Login') {
		    steps {
						dir('') {
						sh 'docker login -u chandanbms -p dodderi123'
						}
					}
		}
		
		stage('Push') {
		    steps {
						dir('') {
						sh 'docker push chandanbms/addressbook:latest'
						}
					}
		}
		
		stage('logout') {
		    steps {
						dir('') {
						sh 'docker logout'
						}
					}
		}
		
		stage('Deployment') {
		    steps {
						dir('') {
						sh 'kubectl apply -f /root/chandan/manifest/deployment.yml'
						}
					}
		}
		
                stage('Email') {
			steps {
						emailext attachLog: true, body: 'The status of the build can be obtained from the build log attached', subject: 'STATUS: $PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'chandanbms@gmail.com'
				}
		}		
		
		}
		
}

