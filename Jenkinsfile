pipeline{
 agent any
  tools{
    maven 'Maven'
  }
 
 environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')


  
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
 
  stage ('MOVING Dockerfile TO ANSIBLE') {
    steps{
    sshPublisher(publishers: [sshPublisherDesc(configName: 'ANSIBLE', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''docker image rm -f yaswanth_demo
    cd /home/ansible/opt/docker
     docker build -t yaswanth/yaswanth_demo .
     ''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//home//ansible//opt//docker', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'dockerfile')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
      }
    }
    
    stage('Login') {
     steps {
				   sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			    }
		   }

    stage('Push') {
      steps {
				      sh 'docker push yaswanth650/yaswanth_demo:latest'
			      }
		    }

    stage ('creating docker container') {
     steps{
       sshPublisher(publishers: [sshPublisherDesc(configName: 'ANSIBLE', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: ''' cd /home/ansible/prod/playbook
         ansible-playbook create_docker_container.yml''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
      }
     }
}
}
