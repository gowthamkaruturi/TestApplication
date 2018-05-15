pipeline {
  agent {
  node{
  label 'master'
  }
  }
  tools {
    maven 'MAVEN_HOME'
  }
  stages{
  	stage('test'){
  	    steps{
  	        sh 'mvn test'
  	        junit '**/target/surefire-reports/*.xml'
  	    }

  	}

    stage('build'){
    steps{
        sh 'mvn -version '
          sh 'mvn compile package'
        }
      }
      stage('deploy'){
    steps{
    sh  'cp target/application_*.jar /var/www/html/rectangles/all'
      }
    }
    }
    post{
    always{
      archiveArtifacts artifacts: 'target/*.jar' , fingerprint :true
    }
    }
  }
