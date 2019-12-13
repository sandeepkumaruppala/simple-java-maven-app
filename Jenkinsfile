pipeline {
  agent none
  environment{
        image_tag = "amitp30/java:app-test"
        app = "java_test"
    }
  stages {
    stage('compile and package') {
		agent {
			docker {
				image 'maven:3-alpine'
				args '-v /root/.m2:/root/.m2'
			}
		}
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }
    stage('test') {
	  agent {
			docker {
				image 'maven:3-alpine'
				args '-v /root/.m2:/root/.m2'
			}
		}
      steps {
        sh 'mvn test'
      }
    }
	stage('Build Docker image') {
			agent any
            steps {
                echo 'Building docker image...!!'
                sh 'docker build -t $image_tag .'
            }
			post {
				always {
					junit 'target/surefire-reports/*.xml'
				}
			}
    }
	stage('Push image'){
				agent any
                steps{
                        script{
                                withDockerRegistry(credentialsId: 'docker-hub-amitp30', url: '') {
                                        sh 'docker push $image_tag'
                                }
                        }
                }
        }
        stage('Anchore_Security_Scan'){
				agent any
                steps{
                        echo 'keep ancore scanner here'
                        sh 'echo "$image_tag" ${WORKSPACE}/Dockerfile > anchore_images'
                        anchore 'anchore_images'
                }
        }
        stage('deploy'){
				agent any
                steps{
                        sh 'docker run -dit -p 8080:8080 --name $app $image_tag'
                }
        }
        stage('e2e-tests'){
				agent any
                steps{
                        sh 'echo "add end to end test here"'
                }
        }
        stage('Clean_up'){
				agent any
                input {
                                 message 'please provide approval for cleaning_up the application'
                        }
                steps{
                        sh 'docker stop $app && docker rm $app'
                }
        }
  }

}
