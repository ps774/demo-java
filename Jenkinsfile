pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'd60190d7-b182-4f05-9f7c-f1defae5b9fc', url: 'https://github.com/ps774/demo-java.git'
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war target/myweb2.war"
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb2.war  ec2-user@10.1.3.98:/opt/tomcat9/webapps/
                    
                    ssh ec2-user@10.1.3.98 /opt/tomcat9/bin/shutdown.sh
                    
                    ssh ec2-user@10.1.3.98 /opt/tomcat9/bin/startup.sh
                
                """
            }
            
            }
        }
    }
