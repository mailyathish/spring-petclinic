    pipeline {
        agent any
        
        tools
        {
        maven "Maven"
        }

        environment {
        DOCKER_RUN  = 'docker run -p 8080:8080 -d --name my-app rajuyathi/petclinic-spinnaker-jenkins:latest' 
        }

        stages {


            //  GIT checkout
        stage('checkout') {
            steps {
                
                    git branch: 'master', url: 'https://github.com/mailyathish/spring-petclinic.git'
                
            }
            }
    

        //   Maven build 
        stage('Build Application') { 
            steps {
                echo '=== Building Petclinic Application ==='
                sh 'mvn -B -DskipTests clean package' 
                }
            }

        // Maven Junit - Test
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

        // Performance test using JMeter
                stage('Performance Test') {
                    steps {
                        sh 'mvn verify'
                    
                }
                }
    //  Docker Build
        stage('Docker Build and Tag') {
            steps {
                
                

                sh 'docker build -t  rajuyathi/petclinic-spinnaker-jenkins:latest .' 
                    
                
            }
            }

    //  Pushing the Docker image to Docker Hub 
        stage('Push Docker Image'){
        steps {
        
        withCredentials([string(credentialsId: 'DockerPWDS', variable: 'DockerPass')]) {
            // some block

        
        sh 'docker login -u rajuyathi -p ${DockerPass}'
        sh 'docker push rajuyathi/petclinic-spinnaker-jenkins:latest'
        }
    }
    }

    // Running the Container on Remote AWS - Instance.
    stage('Run Container on Dev Server'){
        steps {
            sshagent(['AWSLogin']) {
                // sh 'docker run -p 9090:9090 -d --name my-app rajuyathi/petclinic-spinnaker-jenkins'
                sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.35.179 docker rm -f my-app || true"
                sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.35.179 ${DOCKER_RUN}"
            }

        }
        }
    }
    }

