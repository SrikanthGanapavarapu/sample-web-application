pipeline{

      agent {
                docker {
                image 'maven:3-openjdk-11'

                }
            }
        
        stages{

              stage('Quality Gate Status Check'){
                  steps{
                      script{
    			withSonarQubeEnv('sonarqube') {
      				sh "mvn clean sonar:sonar \
      				-D sonar.login=admin \
      				-D sonar.password=admin \
      				-D sonar.projectKey=sonar-jenkins \
      				-D sonar.exclusions=vendor/**,resources/**,**/*.java \
      				-D sonar.host.url=http://54.242.167.180:9000/"
    			}
			      timeout(time: 1, unit: 'HOURS') {
			      def qg = waitForQualityGate()
				      if (qg.status != 'OK') {
					   error "Pipeline aborted due to quality gate failure: ${qg.status}"
				      }
                    		}
		    	    sh "mvn clean install"
		  
                 	}
               	 }  
              }	
		
            }	       	     	         
}
