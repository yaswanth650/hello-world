pipeline{
 agent any
  tools{
    maven 'Maven'
  }
  
stages{
    stage('Initialize'){
      steps{
        sh '''
              
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            '''
      }
    }
    
   stage ('Build') {
      steps {
      sh 'mvn clean package'
      }
   }
  
  stage ('MOVING ARTIFACTS TO ANSIBLE') {
    steps{
      sshPublisher(publishers: [sshPublisherDesc(configName: 'ANSIBLE', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//home//ansible//opt//docker', remoteDirectorySDF: false, removePrefix: '/webapp/target', sourceFiles: '**/*war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
    }
  }
}
}
