pipeline {
    agent {
        kubernetes {
            inheritFrom 'jenkins-jenkins-agent'
            idleMinutes 5
        }
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
        
        stage('Maven Clean') {
            steps {
                sh "mvn clean"  	 
            }
        }
        
        stage('Maven Build') {
            steps {
                sh "mvn compile"  	 
            }
        }
        
        stage("Unit Test") {          	 
            steps {  	 
                sh "mvn test"          	 
            }
        }
        
        stage("Unit validate") {          	 
            steps {  	 
                sh "mvn validate"          	 
            }
        }
        
        stage("Maven Package") {
            steps {
                sh "mvn package" 
            }
        }
        stage("list files") {
            steps {
                sh "ls -l *" 
            }
        }
    }
    
    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}
