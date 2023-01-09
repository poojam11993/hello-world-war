pipeline {
    agent {label 'slave2'}
    stages {
        stage('my Build') {
            steps {
                sh "echo ${BUILD_VERSION}"
                sh 'docker build -t tomcat_build:${BUILD_VERSION} --build-arg BUILD_VERSION=${BUILD_VERSION} .'
            }
        }  
        stage('publish stage') {
            steps {
                sh "echo ${BUILD_VERSION}"
                sh 'docker login -u puja15 -p Saanvi@26'
                sh 'docker tag tomcat_build:${BUILD_VERSION} puja15/mytomcat:${BUILD_VERSION}'
                sh 'docker push puja15/mytomcat:${BUILD_VERSION}'
            }
        } 
        stage( 'my deploy' ) {
        agent {label 'master'} 
            steps {
               sh 'docker pull puja15/mytomcat:${BUILD_VERSION}'
               sh 'docker rm -f mytomcat'
               sh 'docker run -d -p 8080:8080 --name mytomcat puja15/mytomcat:${BUILD_VERSION}'
            }
        }    
    } 
}
