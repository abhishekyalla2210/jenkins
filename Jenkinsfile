pipeline{
        agent{
                
                label 'demo'
                
        }
        environment {
                course = "devsecops"
                duration = "6 months"
                PROJECT = "roboshop"
       
        

        }
		  parameters {
			string(name: 'NAME', defaultValue: 'Abhishek', description: 'Enter your age')
			choice(name: 'ENV', choices: ['dev', 'stage', 'prod'], description: 'Select environment')
			booleanParam(name: 'DEPLOY', defaultValue: true, description: 'Deploy application?')
    }
        stages {
                stage('Build') {
	        when {
			expression { "$params.DEPLOY" == "true" }
												
			}
                        steps {
                                script{
                                        withAWS(credentials: 'aws-cred', region: 'us-east-1') {
                                        sh '''
                                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 807291695422.dkr.ecr.us-east-1.amazonaws.com
                                        docker build -t roboshop/catalogue .
                                        docker tag roboshop/catalogue:latest 807291695422.dkr.ecr.us-east-1.amazonaws.com/roboshop/catalogue:latest
                                        docker push 807291695422.dkr.ecr.us-east-1.amazonaws.com/roboshop/catalogue:latest
                                        '''
                                        }
                                }
                        }
                }
                stage('json ') {
                        steps {
                                script {
                                        def packageJSON = readJSON file: 'package.json'
                                         echo "${packageJSON.version}"
                                }
                              

                               

                                
                        }
                }
        }

        post{
                always {
                        echo "i run always no matter the result"
                }
                success {
                        echo ' its a successful'
                }

                failure {
                        echo ' its a failure'
                }
                
        }
} 
