﻿pipeline {
 
 agent any
 
 tools{
   maven 'Jenkins-Maven'
 }
 
 environment{
    CODE_VERSION='1.0.0'
    GIT_CREDENTIALS = credentials('global-git-credentials')
    JENKINS_CREDENTIALS = credentials('global-jenkins-credentials')    
 }
 
   //NB:- In Linux we need to execute sh 'echo Executing stage - Build & Create Artifact...' /  sh 'mvn clean install' and in windows we can use bat like bat 'mvn -version'
   stages {        
     stage('Build & Create Artifact') { 
        when {               
           expression { 
		env.BRANCH_NAME == 'master' || env.BRANCH_NAME == 'dev' 
	   }
        }  
	steps {
	  echo '>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Executing stage - Build & Create Artifact >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>'
	  //Using Jenkins Environment Variable
	  echo "Building version ${CODE_VERSION} and will be deploying with ${JENKINS_CREDENTIALS}"
	  echo "PATH ====> ${PATH}"
          echo "M2_HOME ====> ${M2_HOME}"
	  bat 'mvn -version'
        }
     }
     
     stage('Deploy Artifact') { 
        when {               
           expression { 
		env.BRANCH_NAME == 'master' || env.BRANCH_NAME == 'dev' 
	   }
        }     
	steps {
	  echo '>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Executing stage - Deploy Artifact >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>'	  
	  //Using Jenkins Credentials only for a particular stage
	  withCredentials([
	      usernamePassword(credentialsId: 'global-jenkins-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')
	  ]){
		//https://wiki.jenkins.io/display/JENKINS/Credentials%20Binding%20Plugin
		echo "Deploying to Jenkins Server with ${USERNAME} and ${PASSWORD}"
	  }
        }
     }     
   }//stages
   
   post{
     always{
	echo '>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Executing post handler - always >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>'
        mail to: 'maroof.siddique2013@gmail.com',
        subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - ${currentBuild.result} !",
        body: "Please find the build and log details at ${env.BUILD_URL} \n  Log file is available at ${env.JENKINS_HOME}/jobs/${env.JOB_NAME}/branches/${env.BRANCH_NAME}/builds/${env.BUILD_NUMBER}/log" 	
     }
     
     success{
        echo '>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Executing post handler - success >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>'
        echo "${env.JOB_NAME} executed successfuly..."	
     }
     
     failure{
       echo '>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Executing post handler - failure >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>'
       echo "${env.JOB_NAME} failed to execute successfuly..."	       
     }
   
   }//post
   
}//pipeline