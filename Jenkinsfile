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
            semgrep/semgrep semgrep ci  security-audit secrets'''
      }
    }
    
 stage ('Build') {
      steps {
      sh 'mvn clean package'
      }
   }
}
}
