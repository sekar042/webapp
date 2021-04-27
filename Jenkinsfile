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
sh 'OWASPDC_DIRECTORY="$HOME/OWASP-Dependency-Check"'
sh 'DATA_DIRECTORY="$OWASPDC_DIRECTORY/data"'
sh 'REPORT_DIRECTORY="$OWASPDC_DIRECTORY/reports"'
sh 'docker run --rm \
    --volume $(pwd):/src \
     --volume "$DATA_DIRECTORY":/usr/share/dependency-check/data \
     --volume "$REPORT_DIRECTORY":/report \
     owasp/dependency-check \
     --scan /src \
     --format "ALL" \
     --project "My OWASP Dependency Check Project" \
     --out /report'

}
} 
	 
stage ('SAST'){
   steps {
   withSonarQubeEnv('sonar'){
       sh 'mvn sonar:sonar'
       sh 'cat target/sonar/report-task.txt'
   }
   
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
	 
stage ('DAST'){
     steps {
	  sshagent(['zap']){
	   sh 'ssh -o StrictHostKeyChecking=no ubuntu@18.222.35.8 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://3.16.203.173:8080/webapp" || true'
	  }
	
	}

} 
	 
 }
}
