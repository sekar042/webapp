pipeline {
 agent any
 tools {
   maven 'Maven'
 }
 stages {
   stage ('Initialize') {
   steps {
     sh '''
         echo "PATH = $(PATH)"
         echo "M2_HOME = ${M2_HOME}"
        '''
   }
   
   }
  stage ('Check-Git-Secrets') {
	     steps {
       sh 'rm trufflehog || true'
       sh 'docker pull gesellix/trufflehog'
       sh 'docker run -t gesellix/trufflehog --json https://github.com/sekar042/webapp.git > trufflehog'
       sh 'cat trufflehog'
}
}
   stage ('Source Composition Analysis'){
    steps {
       sh 'rm owasp*' || true
       sh 'wget https://github.com/sekar042/webapp/owasp-dependency-check.sh'
       sh 'chmod +x owasp-dependency-check.sh'
       sh 'bash owasp-dependency-check.sh'
    }
} 
	 
	 
   stage ('Build') {
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
