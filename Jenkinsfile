pipeline {
    
    agent any
    
	parameters{
	    
	    string(name: "homol-server-folder", defaultValue: "/apps/webapps-homol", description: "stagging environment to deploy java web application")
	    string(name: "prod-server-folder", defaultValue: "/apps/webapps-prod", description: "production environment to deploy java web application")
	    
	}

	triggers{
	    pollSCM("* * * * *")
	}

    stages {
    
        stage("build") {
            steps{
                sh "mvn clean package"
            }
            post {
				success{
					echo "Build completed with success!"               
	                archiveArtifacts artifacts: "**/target/*.war"
				}
				failure{
				    echo "build completed with erros. Check the build ${BUILD_NUMBER} logs."
				}

            }

        }
        
        stage("Deployments to stagging and production"){

			parallel{
				stage("Deploy to Stagging") {
		        	steps {
		        	    sh "cp **/target/*.war /apps/webapps-homol/"
		        	}
		        	post{
		                success{
		                    echo "build ${BUILD_NUMBER} was deployed to stagging."
		        	        mail to:"thiaggom@gmail.com", 
		        	        subject:"${currentBuild.fullDisplayName}", 
		        	        body: "Phase of the job complete! You must manually approve this build at the folloing url:${BUILD_URL} to deploy to production."
		                }
		                
		                failure{
		                    echo "build ${BUILD_NUMBER} was completed with erros. Check the build logs"
		                }
		        	}
				                     
				}
				
				stage("Deploy to Production") {
				
					steps{
					    timeout(time:5, unit:"DAYS") {
					    	input message: "Approved to PRODUCTION Deployment?"                    
					    }
						
						sh "cp **/target/*.war /apps/webapps-prod/"
					}
					post{
		        	    success {
		        	        mail to:"thiaggom@gmail.com", 
		        	        subject:"${currentBuild.fullDisplayName}", 
		         	        body: "Deploy of build ${BUILD_NUMBER} was deployed to production successfully!"
		 					echo "deploy successfully!"  
		        	    }
					    
					}

				                     
				}

			    
			}

                          
        }


    }

}
