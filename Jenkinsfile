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
    sh "mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}"

    sh  "cp target/application_*.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}"
      }
    }

/*stage('running on centos'){
	agent {
	label 'CentOS'
	}
	steps{

	sh "wget http://gowthamkaruturi1.mylabserver.com/rectangles/all/application_${env.BUILD_NUMBER}.jar"
	sh "java -jar application_${env.BUILD_NUMBER}.jar 3 4"
	}
    }*/


    stage("running on debian")
     {
    agent
    {
    docker 'openjdk:8u121-jre'	
	}
	steps{
	sh "wget http://gowthamkaruturi1.mylabserver.com:80/rectangles/all/${env.BRANCH_NAME}/application_${env.BUILD_NUMBER}.jar"
	sh "java -jar application_${env.BUILD_NUMBER}.jar 3 4"
	}
	}
	stage("promote to green")
	{
	agent {
	label 'apache'	
	}
	when {
	branch 'master'
	}
	steps{
	sh "cp /var/www/html/rectangles/all/${env.BRANCH_NAME}/application_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/application_${env.BUILD_NUMBER}.jar "
	}
	}
	stage("promote development to master")
	{
	agent {
	label 'apache'
	}
	 when {
	branch 'develop'
	}
	steps{
	echo 'stashing local changes'
	sh 'git stash '
	echo "checking out development"
	
	sh 'git checkout develop'
	
	echo "cheking out the master branch"
	sh 'git checkout master'
	echo "merging development to master branch"
	
	sh 'git merge development'
	
	echo "pushing to origin:master"
	
	sh 'git push master'
	}
	}

	}

  }
