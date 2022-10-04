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
	}
}
