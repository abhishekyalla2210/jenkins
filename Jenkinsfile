pipeline{
        agent{
                
                label 'demo'
                
        }
        environment {
                course = "devsecops"
                duration = "6 months"
       
        

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
}