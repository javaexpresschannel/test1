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
               mvn sonar:sonar \
  			-Dsonar.projectKey=springdboo_docker_k8s \
  			-Dsonar.host.url=http://3.125.6.250:9000 \
 			 -Dsonar.login=ea89ea5db609e2458009ffe52ce8f9033fa6f042
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
        
    	stage("kubernetes deployment"){
    		steps {
                sh 'kubectl apply -f k8s-spring-boot-deployment.yml'
            }
    	} 
    }
}
