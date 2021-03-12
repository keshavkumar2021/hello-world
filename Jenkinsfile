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
   
    stage ('Checking Git Secrets'){
     steps {
      sshPublisher(publishers: [sshPublisherDesc(configName: 'gcp-security-tooling', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''rm trufflehog || true;
docker run dxa4481/trufflehog --json https://github.com/keshavkumar2021/hello-world.git > trufflehog;
cat trufflehog''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
     }
   }
   
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
   
   stage ('Snyk-Scanning'){
    steps{
snykSecurity failOnIssues: false, snykInstallation: 'snyk', snykTokenId: 'snyk-api-token'
    }
   }
   
    stage ('Deploy-to-Tomcat'){
     steps {
sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: 'webapp/target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
     }
     } 
   
    stage ('Executing-Ansible-playbooks'){
     steps {
sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''ansible-playbook -i /opt/docker/hosts /opt/docker/create-simple-devops-image.yml --limit localhost;
ansible-playbook -i /opt/docker/hosts /opt/docker/create-simple-devops-project.yml --limit 172.31.25.237''', execTimeout: 12000000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
     }
     } 
   

   
  }
}
