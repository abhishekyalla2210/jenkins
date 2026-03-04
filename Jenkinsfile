pipeline{
        agent{
                
                label 'demo'
                
        }
        stages {
                stage('Build') {
                        steps {
                                echo "building"
                        }


                }
                stage('testing') {
                        steps {
                                sh '''

                                   echo "testing the sh format"

                                '''
                        }
                }
        }
}