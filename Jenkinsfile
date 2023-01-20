pipeline {
    agent {label 'build_Server'}
    stages {
        stage('my Build') {
            steps {
                sh 'docker build -t tomcat_build:${BUILD_NUMBER} .' 
            }
        }  
        stage('publish stage') {
            steps {
                sh "echo ${BUILD_NUMBER}"
                withCredentials([usernamePassword(credentialsId: 'Dockerhub', passwordVariable: 'DockerhubPassword', usernameVariable: 'DockerhubUser')]) {
                sh "docker login -u ${env.DockerhubUser} -p ${env.DockerhubPassword}"
                sh 'docker tag tomcat_build:${BUILD_NUMBER} puja15/mytomcat:${BUILD_NUMBER}'
                sh 'docker push puja15/mytomcat:${BUILD_NUMBER}'
                }    
            }
        }
        stage( 'my deploy' ) {
        agent {label 'test'} 
            steps {
               sh 'docker pull  puja15/mytomcat:${BUILD_NUMBER}'
               sh 'docker rm -f mytomcat'
               sh 'docker run -d -p 8080:8080 --name mytomcat puja15/mytomcat:${BUILD_NUMBER}'
            }
        }    
        stage("build & SonarQube analysis") {
        agent {label 'test'}
           steps {
              withSonarQubeEnv('Sonarqube-8.3') {
                sh 'mvn clean package sonar:sonar'
              }
            }
        }   
          stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
    } 
}
