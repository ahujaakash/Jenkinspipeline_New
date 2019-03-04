pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '172.31.30.167', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '172.31.20.143', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
                        sh "scp -i /home/akash/jenkins/Jenkins_Pipeline_POC_keyPair.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

         //       stage ("Deploy to Production"){
         //           steps {
         //               sh "scp -i /home/akash/jenkins/Jenkins_Pipeline_POC_keyPair.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
         //           }
                //}
            }
        }
    }
}