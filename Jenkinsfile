pipeline{
 agent any
  tools{
    maven 'Maven'
  }
  stages{
    stage('Initialize'){
      steps{
        sh '''      echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            '''
      }
    }
stage('ARACHNI') {
         steps {
          arachniScanner checks: '*', scope: [pageLimit: 3], url: 'https://65.0.204.162:8443/WebApp/',userConfig: [filename: '/var/lib/jenkins/workspace/HELLOWORLD/myConfiguration.json'],format: 'json'
         }
      }
    stage('Probely') {
                 steps {
                    probelyScan targetId: 'iUo7NcLuyBC3', credentialsId: 'probely', waitForScan: true, stopIfFailed: true, failThreshold: 'high'
               }
           }
   }
}
 
