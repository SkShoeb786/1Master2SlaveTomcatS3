pipeline{
	agent{
		label{
			label'built-in'
			customWorkspace'/mnt/project/'
		}
	}
	stages{
		stage('build war on master'){
			steps{
				sh 'mvn clean install -DskipTests'
			}
		}
		stage('copy master to s3'){
			steps{
				sh 'aws s3 cp /mnt/project/gameoflife-web/target/gameoflife.war s3://sk.buck/'
			}
		}
		stage('parallel'){
			parallel{
				stage('copy s3 to slave1'){
					agent{
						label{
							label'slave1'
						}
					}
					steps{
						sh 'sudo rm -rf /home/ec2-user/webserver/apache-tomcat-9.0.67/webapps/gameoflife*'
						sh 'sudo aws s3 cp s3://sk.buck/gameoflife.war /home/ec2-user/webserver/apache-tomcat-9.0.67/webapps/'
						sh 'sudo /./home/ec2-user/webserver/apache-tomcat-9.0.67/bin/shutdown.sh'
						sh 'sudo /./home/ec2-user/webserver/apache-tomcat-9.0.67/bin/startup.sh'
					}
				}
				stage('copy s3 to slave2'){
					agent{
						label{
							label'slave2'
						}
					}
					steps{
						sh 'sudo rm -rf /home/ec2-user/webserver/apache-tomcat-9.0.67/webapps/gameoflife*'
						sh 'sudo aws s3 cp s3://sk.buck/gameoflife.war /home/ec2-user/webserver/apache-tomcat-9.0.67/webapps/'
						sh 'sudo /./home/ec2-user/webserver/apache-tomcat-9.0.67/bin/shutdown.sh'
						sh 'sudo /./home/ec2-user/webserver/apache-tomcat-9.0.67/bin/startup.sh'
					}
				}
			}
		}
	}
}
