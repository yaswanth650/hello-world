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
    stage ('Build') {
      steps {
      sh 'mvn clean package'
      }
   }
  stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no **/target/*.war ubuntu@13.232.44.90:/prod/apache-tomcat-10.1.30/webapps/Webapp.war'
              }      
           }       
       }
stage('ARACHNI') {
         steps {
          arachniScanner checks: '*', scope: [pageLimit: 3], url: 'https://13.232.44.90:8443/Webapp',userConfig: [filename: '/var/lib/jenkins/workspace/HELLOWORLD/myConfiguration.json'],format: 'json'
         }
      }
   }
}
 
