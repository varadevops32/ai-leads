pipeline {
    agent any

    stages {
        stage('CodeCheckout') {
            steps {
                git branch: 'main', url: 'https://github.com/varadevops32/ai-leads.git'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Tomcat Deployment') {
            steps {
                sshagent(['d286805b-1a3f-4e51-8e24-22485a0e09a7']) {
             sh """
                        scp -o StrictHostKeyChecking=no  target/ai-leads.war ec2-user@172.31.13.143:/opt/tomcat10/webapps/
                        ssh ec2-user@172.31.13.143    /opt/tomcat10/bin/shutdown.sh
                        ssh ec2-user@172.31.13.143    /opt/tomcat10/bin/startup.sh
                    
                    """

}
            }
        }
    }
} 
