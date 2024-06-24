pipeline {
   agent any 
   tools {
        maven "MAVEN"
    }
    stages {
		stage('Checkout') { 
            steps {
                git 'https://github.com/javaexpresschannel/test1.git'
            }
        }
        stage('Compile') { 
            steps {
                sh "mvn clean compile"
            }
        }
        stage('Build') { 
            
            steps {
                sh "mvn package"
            }
        }        
        stage('sonar') { 
            steps {
                sh '''
mvn clean verify sonar:sonar \
  -Dsonar.projectKey=springboot \
  -Dsonar.host.url=http://3.67.133.94:9000 \
  -Dsonar.login=sqp_425393f1bdb571ad567c5bcbbc45e0a4ba3cd1b2
                '''
            }
        } 
        stage('Docker Build'){
            steps {
                sh 'docker build -t javaexpress/springboot_docker_k8s_sonar:latest .'
            }
        }   
        stage('Docker Login'){
            
            steps {
                 withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
        			sh 'docker login -u javaexpress -p $PASSWORD'
   		 		}
            }                
        }
        stage('Docker Push'){
            steps {
                sh 'docker push javaexpress/springboot_docker_k8s_sonar:latest'
            }
        } 
        
    	
    }
}
