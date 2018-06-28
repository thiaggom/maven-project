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
        	    
        	    success {
//        	        mail to:"thiaggom@gmail.com", 
//        	        subject:"${currentBuild.fullDisplayName}", 
//        	        body: "Phase of the job complete! You must manually approve this build at the folloing url:${BUILD_URL} to deploy to production."
					echo "Deploy to staging server finished successfully!"  
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
			    
        	    success {
//        	        mail to:"thiaggom@gmail.com", 
//        	        subject:"${currentBuild.fullDisplayName}", 
 //       	        body: "Deploy of build ${BUILD_NUMBER} was deployed to production successfully!"
 					echo "deploy successfully!"  
        	    }
			    
			    failure{
			        echo "Deploy failed!"
			    }
			    
			}
		                     
		}


    }

}
