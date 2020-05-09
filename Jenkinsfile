pipeline{
        agent none
        environment{
                image_tag = 'sandeepkumaruppala/expert:java'
                app_name = 'java_app'
        }
        stages{
                stage('build_java'){
                        agent{
                                docker{
                                        image 'maven:3-alpine'
                                        args '-v $HOME/.m2:/root/.m2'
                                }
                        }
                        steps{
                             sh 'mvn -B -DskipTests clean package'   
                        }

                }
                stage('unit_test'){
                        agent{
                                docker{
                                        image 'maven:3-alpine'
                                        args '-v $HOME/.m2:/root/.m2'
                                }        
                        }
                        steps{
                                sh 'mvn test'
                        }
                        post{
                                success{
                                        junit 'target/surefire-reports/*.xml'
                                }
                        }
                }
                stage('build_docker'){
                        agent{
                                label 'bash-docker'
                        }
                        steps{
                                sh 'docker build -t ${image_tag} .'
                                script{
                                        withDockerRegistry(credentialsId: 'dockerhub-sandeep') {
                                                sh 'docker push ${image_tag}'
                                        }
                                }
                        }
                }
                stage('deploy'){
                        agent{
                                label 'bash-docker'
                        }
                        steps{
                                docker run -dit -p 8080:8080 --name ${app_name} ${image_tag}
                        }

                }
                stage('clean_up'){
                        agent{
                                label 'bash-docker'
                        }
                         input{
                                      message 'please approve to cleanup env'
                              } 
                        steps{
                           sh ' docker stop ${app_name} && docker rm ${app_name}'  
                        }
                }

        }
}
