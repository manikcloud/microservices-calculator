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
                    imagePullPolicy: IfNotPresent
                    securityContext:
                      privileged: true
            '''
            defaultContainer 'docker'
        }
    }

environment {
        DOCKER_IMAGE = "aksacrops.azurecr.io/calc:${env.BUILD_NUMBER}"
    }    
    tools {
        maven 'my_mvn'
    }
    
    stages {
        // stage("Checkout") {   
        //     steps {               	 
        //         git branch: 'main', url: 'https://github.com/manikcloud/microservices-calculator.git'        	 
        //     }    
        // }
        
        stage('Maven install') {
            steps {
              container('jnlp') {
                sh "mvn clean install"
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
                withCredentials([usernamePassword(credentialsId: 'acr-ops', usernameVariable: 'ACR_USERNAME', passwordVariable: 'ACR_PASSWORD')]) {
                    sh """
                    docker login aksacrops.azurecr.io -u $ACR_USERNAME -p $ACR_PASSWORD
                    """
                }
            }
        }
        // Assuming a stage to build the Docker image:
        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t $DOCKER_IMAGE .
                """
            }
        }
        stage('Push Docker Image to ACR') {
            steps {
                sh """
                docker push $DOCKER_IMAGE
                """
            }
        }                
    }
    
    // post { 
    //     always {  
    //         junit 'target/surefire-reports/*.xml'
    //     }
    // }
}

// az aks update -n aks_ops-mature-mite -g devops-rg-smooth-akita --attach-acr aksacrops