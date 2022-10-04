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
	}
}
