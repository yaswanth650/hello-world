pipeline{
 agent any
  tools{
    maven 'Maven'
  }
 environment {
         SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
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
      stage('Semgrep-Scan') {
          steps {
            sh '''docker pull semgrep/semgrep && \
            docker run \
            -e SEMGREP_APP_TOKEN=$SEMGREP_APP_TOKEN \
            -v "$(pwd):$(pwd)" --workdir $(pwd) \
            semgrep/semgrep semgrep ci '''
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
                sh 'scp -o StrictHostKeyChecking=no **/target/*.war ubuntu@13.200.251.142:/prod/apache-tomcat-10.1.30/webapps/Webapp.war'
              }      
           }       
       }
stage('ARACHNI') {
         steps {
           arachniScanner checks: '*',scope: [pageLimit: 3],format: 'html', url: 'https://13.200.251.142:8443/Webapp/', userConfig:[filename: 'Configuration.json']
         }
      }
  }
}
