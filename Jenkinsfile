pipeline {
 agent any
  tools{
  maven 'M2_HOME'
  }
  stages{
    stage ('Initialize'){
      steps {
       sh '''
            echo "PATH = ${PATH}"
            echo "M2_HOME = ${M2_HOME}"
          '''
      }
    }
   
/*    stage ('Check-Git-Secrets'){
     steps{
      sh 'rm trufflehog || true'
      sh 'docker run gesellix/trufflehog --json https://github.com/keshavkumar2021/webapp-1.git > trufflehog'
      sh 'cat trufflehog'
     }
    } */
   
/*   stage ('Source Composition Analysis'){
    steps{
     sh 'rm owasp* || true'
     sh 'wget "https://raw.githubusercontent.com/keshavkumar2021/webapp-1/master/owasp-dependency-check.sh"'
     sh 'chmod +x owasp-dependency-check.sh'
     sh 'bash owasp-dependency-check.sh'
    }
   } */
   
    stage ('Build'){
      steps {
       sh 'mvn clean package' 
      }
    }
   
  /*  stage ('Deploy-to-Tomcat'){
     steps {
      sshagent(['tomcat']) {
       sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.145.211.65:/prod/apache-tomcat-8.5.63/webapps/webapp.war'
      }
     }
   } */
   
  }
}
