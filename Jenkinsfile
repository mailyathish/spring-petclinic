pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }

    stages {
    stage('checkout') {
           steps {
             
                git branch: 'main', url: 'https://github.com/mailyathish/spring-petclinic.git'
             
          }
        }
    }

}

