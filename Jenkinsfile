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
  -Dsonar.host.url=http://15.206.74.52:9000 \
  -Dsonar.login=sqp_f0d3d5e221af10211f76eb788b51dc3ef0f614cd
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
