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

    stage('Test Application') {
            steps {
                echo '=== Testing Petclinic Application ==='
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

            stage('Performance Test') {
                steps {
                    sh 'mvn verify'
                
            }
            }

      stage('Docker Build and Tag') {
           steps {
              
               
               // sh '/Applications/Docker.app/Contents/Resources/bin/docker build -t rajuyathi/calculator:latest .' 
               sh 'docker build -t  rajuyathi/petclinic-spinnaker-jenkins:latest .' 
                
               
          }
        }

    stage('Push Docker Image'){
	steps {
	
	withCredentials([string(credentialsId: 'DockerPWDS', variable: 'DockerPass')]) {
    	// some block

	//sh '/Applications/Docker.app/Contents/Resources/bin/docker login -u rajuyathi -p ${DockerPass}'
   sh 'docker login -u rajuyathi -p ${DockerPass}'

	
	 // some block
	//sh '/Applications/Docker.app/Contents/Resources/bin/docker push rajuyathi/calculator:latest'
   sh 'docker push rajuyathi/petclinic-spinnaker-jenkins:latest'
	}
   }
   }






    }
 }

