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


    }

}
