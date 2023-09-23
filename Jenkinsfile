pipeline {
    agent {
        kubernetes {
            inheritFrom 'jenkins-jenkins-agent'
            idleMinutes 5
            yaml '''
              apiVersion: v1
              kind: Pod
              spec:
                restartPolicy: Never
                containers:
                  - name: docker
                    image: aksacrops.azurecr.io/dind-azcli:v1
                    command: ["tail", "-f", "/dev/null"]
                    imagePullPolicy: IfNotPresent
                    securityContext:
                      privileged: true
                  - name: dockerh
                    image: aksacrops.azurecr.io/kbctl-helm:v1
                    command: ["tail", "-f", "/dev/null"]
                    imagePullPolicy: IfNotPresent
                    securityContext:
                      privileged: true                      
            '''
            defaultContainer 'jnlp'
        }
    }

    environment {
        DOCKER_IMAGE = "aksacrapp.azurecr.io/calc:${env.BUILD_NUMBER}"
    }    
    tools {
        maven 'my_mvn'
    }

 
  stages {
        stage("Checkout") {   
            steps {               	 
                git branch: 'main', url: 'https://github.com/manikcloud/microservices-calculator.git'        	 
            }    
        }
        
        stage('Maven install') {
            steps {
              container('jnlp') {
                sh "mvn install"
              }	 
            }
        }
        
        // stage('Maven Build') {
        //     steps {
        //         sh "mvn compile"  	 
        //     }
        // }
        
        // stage("Unit Test") {          	 
        //     steps {  	 
        //         sh "mvn test"          	 
        //     }
        // }
        
        // stage("Unit validate") {          	 
        //     steps {  	 
        //         sh "mvn validate"          	 
        //     }
        // }
        
        // stage("Maven Package") {
        //     steps {
        //         sh "mvn package" 
        //     }
        // }
        stage("list files") {
            steps {
                sh "ls -l *" 
            }
        }

        stage('Docker Login to ACR') {
            steps {
               withCredentials([usernamePassword(credentialsId: 'aksacrapp', usernameVariable: 'ACR_USERNAME', passwordVariable: 'ACR_PASSWORD')]) {
                    container('docker') {
                        sh """
                        docker login aksacrapp.azurecr.io -u $ACR_USERNAME -p $ACR_PASSWORD
                        """
                  }
              }
            }
        }
        // Assuming a stage to build the Docker image: 
        stage('Build Docker Image') {
            steps {
                // container('docker') {
                    sh """
                    docker build -t $DOCKER_IMAGE .
                    """
                // }
            }
        }
        stage('Push Docker Image to ACR') {
            steps {
                container('docker') {
                    sh """
                    docker push $DOCKER_IMAGE
                    """
                }
            }
        }                
    
    //  stage('CD') {
    //       steps {
    //           container('dockerh') { // or 'docker-kbctl-helm', depending on which container you want to use
    //               sh "helm upgrade --install prd-java-calc golden-chart/ -f java-calc/values.yaml"
    //               sh "helm ls -A"
    //           }
    //       }
    //   }
    }
}
