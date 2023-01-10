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
                sh 'docker tag tomcat_build:${BUILD_VERSION} puja15/mytomcat:${BUILD_VERSION}'
                sh 'docker push puja15/mytomcat:${BUILD_VERSION}'
                }    
            }
        }
        stage( 'my deploy' ) {
        agent {label 'test'} 
            steps {
               sh 'docker pull  puja15/mytomcat:${BUILD_NUMBER}'
               sh 'docker rm -f mytomcat'
               sh 'docker run -d -p 8080:8080 --name mytomcat  puja15/mytomcat:${BUILD_NUMBER}'
            }
        }    
    } 
}
