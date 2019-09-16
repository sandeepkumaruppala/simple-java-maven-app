pipeline {
  agent { label 'maven' } 
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
