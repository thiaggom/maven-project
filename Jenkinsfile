pipeline {
    
    agent any
    
    stages {
        stage("build") {
            steps{
                build job: "package"
            }
        }
        
        stage("Deploy to Staging"){
        	
        	steps {
        	    
        	    build job: "deploy-to-staging"
        	    
        	}
        	
        	post {
        	    
        	    always {
        	        mail to:"thiaggom@gmail.com"  
        	    }

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
			    
        	    always {
        	        mail to:"thiaggom@gmail.com"  
        	    }
			}
		                     
		}


    }

}
