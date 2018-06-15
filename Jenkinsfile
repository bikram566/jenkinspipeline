pipeline {
    agent any

    parameters {
         string(name: 'tomcat-staging-server', defaultValue: '54.227.125.216', description: 'Staging Server')
         string(name: 'tomcat-production-server', defaultValue: '34.227.114.149', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
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
                        bat "cp -i /Users/sony/MYKEYS/no1key.pem **/target/*.war ec2-user@${params.tomcat-staging-server}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "cp -i /Users/sony/MYKEYS/no1key.pem **/target/*.war ec2-user@${params.tomcat-production-server}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}