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
  	        junit 'targets/surefire-reports/TEST-RectangleTest.xml'
  	    }

  	}

    stage('build'){
    steps{
        sh 'mvn -version '
          sh 'mvn compile package'
        }
      }
    }
    post{
    always{
      archiveArtifacts artifacts: 'target/*.jar' , fingerprint :true
    }
    }
  }