pipeline {
 agent any
 tools {
   maven 'Maven'
 }
 stages {
   stage ('Initialie') {
   steps {
     sh '''
         echo "PATH = $(PATH)"
         echo "M2_HOME = ${M2_HOME}"
        '''
   }
   
   }
   
   stage ('build') {
    steps {
    sh 'mvn clean package'
    }
   }
  stage ('Deploye to the tomcat file') {
   steps {
     sshagent(['tomcat']) {
            sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.16.203.173:/home/ubuntu/prod/apache/webapps/'
   }
   }

  }
 }
}
