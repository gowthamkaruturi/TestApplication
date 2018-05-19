pipeline {
  agent none
  tools {
    maven 'MAVEN_HOME'
  }
  stages{

  	stage('test'){
    agent {
    label 'apache'

    }
  	    steps{
  	        sh 'mvn test'
  	        junit '**/target/surefire-reports/*.xml'
  	    }

  	}

    stage('build'){

    agent {
    label 'apache'

    }
    steps{
        sh 'mvn -version '
          sh 'mvn compile package'
        }
         post{
    success{
      archiveArtifacts artifacts: 'target/*.jar' , fingerprint :true
    }
    }
      }
      stage('deploy'){
      agent {
      label 'apache'

      }
    steps{
    sh  'cp target/application_*.jar /var/www/html/rectangles/all'
      }
    }

/*stage('running on centos'){
	agent {
	label 'CentOS'
	}
	steps{

	sh "wget http://gowthamkaruturi1.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
	sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
	}
    }*/


    stage("running on debian")
     {
    agent
    {
    docker 'openjdk:8u121-jre'
}
steps{
sh "wget http://gowthamkaruturi1.mylabserver.com:8080/rectangles/all/apllication_${env.BUILD_NUMBER}.jar"
sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
 }
}


  }
  }
