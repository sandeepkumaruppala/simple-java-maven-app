pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }

  }
  stages {
    stage('build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }
    stage('sample') {
      steps {
        sh 'echo "hello world"'
      } 
    }
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
