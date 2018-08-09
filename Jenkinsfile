pipeline {
    agent any
    parameters {
        string(name: 'tomcat_dev', defaultValue: '52.91.227.231', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '52.87.203.32', description: 'Production Server')
    }
    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Build') {
            steps {
                sh "mvn clean package"
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
                        sh "scp -vvv -i /home/evgeny/Studying/Jenkins/tomcat-demo-us.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -vvv -i /home/evgeny/Studying/Jenkins/tomcat-demo-us.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}