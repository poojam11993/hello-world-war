pipeline {
  agent {label 'slave2'}
  stages {
    stage ('my build') {
      steps {
        sh "echo ${BUILD_NUMBER}"
        sh 'mvn package'  
        sh 'chmod 777 target'
        sh 'pwd'
        sh 'scp -r /home/slave2/workspace/build deploy/target/hello-world-war-1.0.0.war var/lib/jenkins/workspace/build deploy@172.31.0.129:/opt/apache-tomcat-10.0.27/webapps/'
      }
    }
stage ('my deploy') {
   agent {label 'master'}
   steps {
        sh 'sudo sh /opt/apache-tomcat-10.0.27/bin/shutdown.sh'
        sh 'sleep 2'
        sh 'sudo sh /opt/apache-tomcat-10.0.27/bin/startup.sh'
      }
    }
  }
}

