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
}
