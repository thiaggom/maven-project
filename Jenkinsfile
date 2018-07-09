pipeline {
    agent any
    
    environment {
	    SONAR_HOST_URL = 'https://sonarcloud.io'
		SONAR_AUTH_TOKEN = credentials('sonar-token')
		SONAR_ORGANIZATION= 'thiaggom-github'
    }

    stages {
        stage('SCM') {
            steps {
                git url: 'https://github.com/thiaggom/maven-project.git', branch: 'sonarteste'
            }
        }
        stage('build && SonarQube analysis') {
            steps {
            	//deleteDir()
	        	sh "mvn clean verify sonar -P "+
				  "-Dsonar.organization=${SONAR_ORGANIZATION} "+
				  "-Dsonar.host.url=${SONAR_HOST_URL} "+
				  "-Dsonar.login=${SONAR_AUTH_TOKEN} "            
			}
			post{
			    success{
			        echo "build completed successfully!"
			    }

			}

        }

        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    // Requires SonarQube Scanner for Jenkins 2.7+

                    waitForQualityGate abortPipeline: true
                }
            }
        }
        
    }
}