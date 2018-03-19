pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '52.67.249.213', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.231.174.231', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
             parallel{ 
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -oStrictHostKeyChecking=no -i ~/keys-container/keeaccess.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -oStrictHostKeyChecking=no -i ~/keys-container/keeaccess.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
	    }
        }
    }
}
