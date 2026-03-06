pipeline{
        agent{
                
                label 'demo'
                
        }
        environment {
                course = "devsecops"
                duration = "6 months"
       
        

        }
		  parameters {
			string(name: 'NAME', defaultValue: 'Abhishek', description: 'Enter your name')
			choice(name: 'ENV', choices: ['dev', 'stage', 'prod'], description: 'Select environment')
			booleanParam(name: 'DEPLOY', defaultValue: true, description: 'Deploy application?')
    }
        stages {
                stage('Build') {
                        steps {
                                script{
                                        sh """
                                        echo "whats your name"
                                        echo "i am good at ${course}"

                                        """
                                }


                }
                }
                stage('testing') {
                        steps {
                              

                                echo "testing the sh format"

                                
                        }
                }
        }

        post{
                always {
                        echo "i run always no matter the result"
                }
                success {
                        echo ' its a success'
                }

                failure {
                        echo ' its a failure'
                }
                aborted {
                        echo 'checking the triggers'
                        echo 'checkng again'
                }
        }
} 
