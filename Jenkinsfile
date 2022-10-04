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
						sh 'sudo aws s3 cp s3://sk.buck/gameoflife /home/ec2-user/webserver/apache-tomcat-9.0.67/webapps/'
					}
				}
				stage('copy s3 to slave2'){
					agent{
						label{
							label'slave2'
						}
					}
					steps{
						sh 'sudo aws s3 cp s3://sk.buck/gameoflife /home/ec2-user/webserver/apache-tomcat-9.0.67/webapps/'
					}
				}
			}
		}
	}
}
