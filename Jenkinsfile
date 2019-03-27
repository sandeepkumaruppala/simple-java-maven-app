pipeline {
	agent {
		docker {
			image 'maven:3-alpine'
			args '-v /root/.m2:/root/.m2'
		}
	}
	stages {
		stage('build'){
			steps{
				sh 'mvn -B -DskipTests clean package'
			}	
		}
		stage('test'){
			steps{
				sh 'mvn test'
			}
			post{
				always{
					junit 'target/surefire-reports/*.xml'
				}
			}
		}
		stage('Deliver') { 
            		steps {
                		sh './jenkins/scripts/deliver.sh' 
				input message: 'Finished using the web site? (Click "Proceed" to continue)'
				sh 'echo "hello world"'
           		 }
		}
		stage('test-parallel'){
			parallel{
      				stage('job1'){
        				steps{
          					sh 'echo "I am in job1"'
        				}  
      				}
      				stage('job2'){
        				steps{
          					sh 'echo "I am in job 2"'
        				}  
      				}
   			}
		}	
	}
}
