pipeline {
  agent none
  environment{
  MAJOR_VERSION = 1
  }
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
    sh "if ![ -d '/var/www/html/rectangles/all/${env.BRANCH_NAME}' ]; then mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}; fi"

    sh  "cp target/application_${env.MAJOR_VERSION}_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}"
      }
    }

/*stage('running build on centos'){
	agent {
	label 'CentOS'
	}

	steps{

	sh "wget http://gowthamkaruturi1.mylabserver.com/rectangles/all/application_${env.MAJOR_VERSION}_${env.BUILD_NUMBER}"
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
	sh "wget http://gowthamkaruturi1.mylabserver.com:80/rectangles/all/${env.BRANCH_NAME}/application_${env.MAJOR_VERSION}_${env.BUILD_NUMBER}.jar"
	sh "java -jar application_${env.MAJOR_VERSION}_${env.BUILD_NUMBER}.jar 3 4"
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
	sh "cp /var/www/html/rectangles/all/${env.BRANCH_NAME}/application_${env.MAJOR_VERSION}_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/application_${env.MAJOR_VERSION}_${env.BUILD_NUMBER}.jar "
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
	echo 'setting git user name'
	sh ' git --config user.name gowthamkaruturi@gmail.com '
	echo 'stashing local changes'
	sh 'git stash '
	echo "checking out development"

	sh 'git checkout develop'

  	sh 'git pull origin'
  	

	echo "checking out the master branch"
	sh 'git checkout master'
	echo "merging development to master branch"

	sh 'git merge develop'

	echo "pushing to origin:master"

	sh 'git push origin master'
  
  	echo 'tagging the release'

  sh "git tag application-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
  sh "git push origin application-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
	}
	post {
	    success {
	    emailText (
	    subject : "${env.JOB_NAME} [  ${ env.BUILD_NUMBER} ] Success !",
	    body : """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Success!":</p>
        <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
	    to:"karuturigowtham@gmail.com"
	    )
	    }
	}

	}

	}
	post {
	    failure {
	    emailText (
	    subject : "${env.JOB_NAME} [  ${ env.BUILD_NUMBER} ] failed !",
	    body : """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Failed!":</p>
        <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
	    to:"karuturigowtham@gmail.com"
	    )
	    }
	}

  }
