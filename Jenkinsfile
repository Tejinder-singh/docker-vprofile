pipeline {
    
	agent any
/*	
	tools {
        maven "maven3"
	
*/	
    
	
     environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "18.207.97.112:8081"
        NEXUS_REPOSITORY = "my-docker-repo"
	NEXUS_REPO_ID    = "my-docker-repo"
        NEXUS_CREDENTIAL_ID = "nexuslogin"
        ARTVERSION = "${env.BUILD_ID}"
	imageName = "vprofile"
	DockerImage = ''
    }
	
    stages{
        
        stage('BUILD'){
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

	stage('UNIT TEST'){
            steps {
                sh 'mvn test'
            }
        }

	stage('INTEGRATION TEST'){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }
		
        stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }
	 stage('Build Image'){
	    steps {
		    script {
		           dockerImage = docker.build imageName
		           }		    
		    }   
	      }       	    
               
    }
}
