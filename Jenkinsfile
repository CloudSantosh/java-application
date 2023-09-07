pipeline {
	agent any

	stages {

		stage('Checkout code'){
			steps {
			checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/CloudSantosh/Devops.git']])
			}
		}

		stage('Install Maven Build tool'){
			steps {
				//dir('/home/ubuntu'){
				sh 'wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz'
				sh 'tar -xzvf /var/lib/jenkins/workspace/testpipeline/apache-maven-3.9.4-bin.tar.gz'
				}
			}
		//}

		stage('Compile Sample Application'){
			steps {
				dir('/var/lib/jenkins/workspace/testpipeline/addressbook/addressbook_main'){
				sh '/var/lib/jenkins/workspace/testpipeline/apache-maven-3.9.4/bin/mvn compile'
				}
			}
		}

		stage('Test Sample Application'){
			steps {
				dir('/var/lib/jenkins/workspace/testpipeline/addressbook/addressbook_main'){
				sh '/var/lib/jenkins/workspace/testpipeline/apache-maven-3.9.4/bin/mvn test'
				}
			}
		}
		
		stage('package sample application'){
			steps {
				dir('/var/lib/jenkins/workspace/testpipeline/addressbook/addressbook_main'){
				sh '/var/lib/jenkins/workspace/testpipeline/apache-maven-3.9.4/bin/mvn package'
				}
			}
		}


		stage('Tomcat pre-requisities installation'){
			steps {
				sh 'sudo apt update'
				sh 'sudo apt install openjdk-17-jre -y'
				//sh 'sudo mkdir -p /home/ubuntu/tomcat/tomcat1'
				//sh 'sudo chmod 755 /home/ubuntu/tomcat/tomcat1'
				}
			}

		stage('Install Apache Tomcat Server'){
			steps {
				// dir('/home/ubuntu/tomcat/tomcat1'){
				sh 'wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.80/bin/apache-tomcat-9.0.80.tar.gz'
				sh 'sudo tar -xzvf apache-tomcat-9.0.80.tar.gz'
				sh 'sudo chown -R ubuntu:ubuntu apache-tomcat-9.0.80'
				sh 'sudo cp tomcat-users.xml apache-tomcat-9.0.80/conf/tomcat-users.xml'
				sh 'sudo cp context.xml apache-tomcat-9.0.80/webapps/manager/META-INF/context.xml'
				sh 'sudo sed -i "s/8080/8081/g" apache-tomcat-9.0.80/conf/server.xml'

				//}
			}
		}
		
		stage('Deploy sample application to tomcat server'){
			steps {
				sh 'sudo cp addressbook/addressbook_main/target/addressbook.war apache-tomcat-9.0.80/webapps/'
				// sh 'sudo nohup sh apache-tomcat-9.0.80/bin/startup.sh'

				sh 'sudo runuser -l ubuntu -c "/var/lib/jenkins/workspace/testpipeline/apache-tomcat-9.0.80/bin/startup.sh"'
			}

		}

	}
}