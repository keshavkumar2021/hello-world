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
   
      stage ('Snyk-Scanning'){
    steps{
snykSecurity failOnIssues: false, snykInstallation: 'snyk', snykTokenId: 'snyk-api-token'
    }
   }
   
    stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
      }
    }

    stage ('Build'){
      steps {
       sh 'mvn clean install package' 
      }
    }
   
    stage ('Deploy .war_file to Ansible'){
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
   
    stage ('DAST') {
      steps {
        sshagent(['gcpssh']) {
         sh 'ssh -o  StrictHostKeyChecking=no keshav@35.238.150.17 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://54.82.76.219:8080/webapp/" || true'
        }
      }
    }
   
  }
}
