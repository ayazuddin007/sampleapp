//def tomcatServerIP = '172.31.45.105'
def ansibleServerIP = '172.31.2.16'
def ansiblePlaybook = 'ansible-playbook ansible/p1.yml -i ansible/hosts'
//def ansiblePlaybook = 'ansible-playbook ansible/p2.yml -i ansible/hosts'

pipeline {
    agent any
    environment {
        PATH = "/opt/maven/bin/:$PATH"
    }
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/ayazuddin007/sampleapp'   
            }
        }
        stage('Build') {
            steps {
                sh "mvn clean install"  
            }
        }
        /*stage('Deploy to Tomcat') {
            steps {
                sshagent(['Pipeline-user']) {
                    sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@${tomcatServerIP}:/opt/tomcat/webapps"
                }  
            }
        }*/
        stage('Deploy to Ansible') {
            steps {
                sshagent(['Pipeline-user']) {
                    sh "scp -o StrictHostKeyChecking=no -r ansible target/*.war ec2-user@${ansibleServerIP}:/home/ec2-user"
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@${ansibleServerIP} ${ansiblePlaybook}"
                    
                }  
            }
        }
    
    }
}
