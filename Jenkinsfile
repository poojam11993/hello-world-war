pipeline {
  agent {label 'slave2'}
  stages {
    stage ('my build') {
      steps {
        sh "echo ${BUILD_NUMBER}"
        sh 'mvn deploy'  
        sh 'pwd'
      }
    }
stage ('my deploy') {
   agent {label 'master'}
   steps {
        sh 'curl -u puja.manohar1993@gmail.com:Saanvi@26 -O https://pooja123.jfrog.io/ui/repos/tree/General/libs-release-local/com/efsavage/hello-world-war/${BUILD_NUMBER}/hello-world-war-${BUILD_NUMBER}.war'
        sh 'sudo cp -R hello-world-war-${BUILD_NUMBER}.war /opt/apache-tomcat-10.0.27/webapps/'
        sh 'sudo sh /opt/apache-tomcat-10.0.27/bin/shutdown.sh'
        sh 'sleep 2'
        sh 'sudo sh /opt/apache-tomcat-10.0.27/bin/startup.sh'
      }
    }
  }
}

