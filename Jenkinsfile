pipeline {
    agent any
    
    tools{
    maven 'localMaven'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.188.174.68', description: 'Staging Server')
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
                        sh "scp -i /Users/v5103887/gittomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

            }
        }
    }
}