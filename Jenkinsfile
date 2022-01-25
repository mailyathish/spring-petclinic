pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }

    stages {
    stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/mailyathish/spring-petclinic.git'
             
          }
        }
   


    stage('Build Application') { 
        steps {
            echo '=== Building Petclinic Application ==='
            sh 'mvn -B -DskipTests clean package' 
            }
        }

    }
 }

