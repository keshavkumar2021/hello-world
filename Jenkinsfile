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
       sh 'mvn clean install package' 
      }
    }
   
    stage ('Deploy-to-Tomcat'){
     steps {
sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''ansible-playbook -i /opt/docker/hosts /opt/docker/create-simple-devops-image.yml --limit localhost;
ansible-playbook -i /opt/docker/hosts /opt/docker/create-simple-devops-project.yml --limit 172.31.25.237''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: 'webapp/target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
      }
     } 
   
  }
}
