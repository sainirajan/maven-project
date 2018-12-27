pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '35.164.137.214', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.221.30.216', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package '
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
                        bat "pscp -i C:/development/mytomcat.pem "C:\Program Files (x86)\Jenkins\workspace\fully-automated-build\webapp\target\webapp.war" ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "pscp -i C:/development/mytomcat.pem "C:\Program Files (x86)\Jenkins\workspace\fully-automated-build\webapp\target\webapp.war" ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}