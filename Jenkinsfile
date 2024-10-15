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

 stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@13.201.29.88:/prod/apache-tomcat-10.1.30/webapps/28cc9c71-a9de-4ac7-8c3d-de369909854f.txt.war'
              }      
           }       
    }
}
}
