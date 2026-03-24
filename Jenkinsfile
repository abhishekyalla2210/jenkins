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
                                        docker build -t roboshop/catalogue:latest .
                                        docker tag roboshop/catalogue:latest 807291695422.dkr.ecr.us-east-1.amazonaws.com/roboshop/catalogue:latest
                                        docker push 807291695422.dkr.ecr.us-east-1.amazonaws.com/roboshop/catalogue:latest
                                        '''
                                        }
                               }
                        }
                 }
   
		stage('Install NodeJS') { 
			steps { 
				sh ''' 
				# Install Node.js (Amazon Linux / RHEL) 
				sudo dnf install nodejs -y 
				''' 
				} 
				}
                stage('installing npm') {
                        steps {
                                script {
                                        sh """
                                         npm install
                                        """
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


               stage('SonarScanning')  {
                steps {
                        script {
                        def scannerHome = tool 'sonar2.0'
                        withSonarQubeEnv('sonar-server') {
                                sh "${scannerHome}/bin/sonar-scanner"
                        }
                        }
                }

               stage('Dependabot Security Gate') {
                 environment {
                        GITHUB_OWNER = 'abhishekyalla2210'
                        GITHUB_REPO  = 'jenkins'
                        GITHUB_API   = 'https://api.github.com'
                        GITHUB_TOKEN = credentials('git-token')
                }

                steps {
                        script{
                        /* Use sh """ when you want to use Groovy variables inside the shell.
                        Use sh ''' when you want the script to be treated as pure shell. */
                        sh '''
                        echo "Fetching Dependabot alerts..."

                        response=$(curl -s \
                                -H "Authorization: token ${GITHUB_TOKEN}" \
                                -H "Accept: application/vnd.github+json" \
                                "${GITHUB_API}/repos/${GITHUB_OWNER}/${GITHUB_REPO}/dependabot/alerts?per_page=100")

                        echo "${response}" > dependabot_alerts.json

                        high_critical_open_count=$(echo "${response}" | jq '[.[] 
                                | select(
                                .state == "open"
                                and (.security_advisory.severity == "high"
                                        or .security_advisory.severity == "critical")
                                )
                        ] | length')

                        echo "Open HIGH/CRITICAL Dependabot alerts: ${high_critical_open_count}"

                        if [ "${high_critical_open_count}" -gt 0 ]; then
                                echo "❌ Blocking pipeline due to OPEN HIGH/CRITICAL Dependabot alerts"
                                echo "Affected dependencies:"
                                echo "$response" | jq '.[] 
                                | select(.state=="open" 
                                and (.security_advisory.severity=="high" 
                                or .security_advisory.severity=="critical"))
                                | {dependency: .dependency.package.name, severity: .security_advisory.severity, advisory: .security_advisory.summary}'
                                exit 1
                        else
                                echo "✅ No OPEN HIGH/CRITICAL Dependabot alerts found"
                        fi
                        '''
                    
                }
            }
        }
}
}
        

        post{
                always { 
                          echo "i run  no matter the result"
                }
                success {
                        echo ' its a success'
                }

                failure {
                        echo '    its a failure'
                }
                
         }
}
 