pipeline {
  agent { label 'maven' } 
  stages {
    stage('build') {
      agent { docker 'maven:3-alpine' }
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }
    stage('sample') {
      steps {
        sh 'echo "hello world"'
      } 
    }
   }  
 }
