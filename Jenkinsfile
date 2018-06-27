pipeline {
    
    agent any
    
    stages {
        stage("build") {
            steps{
                sh "mvn clean package"
            }
            post{
                success{
                    echo "Build was succeed. Archiving now..."
                    archiveArtifacts artifacts: "**/target/*.war"
                }

            }
        }
        
        stage("Deploy to Staging"){
        	
        	steps {
        	    
        	    build job: "deploy-to-staging"
        	    
        	}
                          
        }

		stage("Deploy to Production") {
		                     
			steps {
                    
			    timeout( time:5, unit:"DAYS") {
			    	input message: "Approve PRODUCTION deployment? "                     
			    }

				build job: "deploy-to-prod"
                    
			}
			
			post {
			    
			    success{
				    echo "Code successfully deployed to production!"
			    }
			    
			    failure{
			        echo "Deploy failed!"
			    }
			    
			}
		                     
		}


    }

}
