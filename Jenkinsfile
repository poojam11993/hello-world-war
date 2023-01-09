pipeline {
    agent {label 'build server'}
    stages {
        stage('my Build') {
            steps {
                sh 'docker build -t tomcat_build:${BUILD_NUMBER} .' 
            }
        }  
        stage('publish stage') {
            steps {
                sh "echo ${BUILD_NUMBER}"
                sh 'docker login -u puja15 -p Saanvi@26'
                sh 'docker tag tomcat_build:${BUILD_NUMBER} puja15/mytomcat:${BUILD_NUMBER}'
                sh 'docker push puja15/mytomcat:${BUILD_NUMBER}'
            }
        } 
        stage( 'my deploy' ) {
        agent {label 'master'} 
            steps {
               sh 'docker pull  puja15/mytomcat:${BUILD_NUMBER}'
               sh 'docker rm -f mytomcat'
               sh 'docker run -d -p 8080:8080 --name mytomcat  puja15/mytomcat:${BUILD_NUMBER}'
            }
        }    
    } 
}
