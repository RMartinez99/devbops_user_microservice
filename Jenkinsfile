pipeline {
     agent any
     stages {
         stage('GitHub Checkout'){
             steps{
                git credentialsId: 'git-creds', url: 'https://github.com/RMartinez99/devbops_user_microservice'
                // git credentialsId: '07225310-2f34-461a-a477-caa56d951f16', url: 'https://github.com/RMartinez99/devbops_user_microservice/'
             }
         }
         stage('Making Sure the parts work') {
             steps {
                 withEnv(["HOME=${env.WORKSPACE}"]) {
                    sh 'python3 test.py'
                 }
             }
         }
         stage('Loading onto Docker'){
             steps{
                 sh 'docker build -t connrailinfo .'

             }

         }
        stage('Sailing off to Docker...'){
            steps{
                withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
                    sh "docker login -u rm267 -p placeholder"
        
                    }
            
                sh 'docker push rm267/connrailinfo'
            }
        }
        stage('Container Execution, on private EC2'){
            steps{
                def dockerRm = 'docker rm -f devbops_user'
                def dockerRmI = 'docker rmi rm267/devbops_user'
                def dockerRun = 'docker run -p 8092:80 -d --name devbops_event rm267/devbops_user'
                sshagent(['docker-server']) {
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@34.239.250.200 ${dockerRm}"
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@34.239.250.200 ${dockerRmI}"
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@34.239.250.200 ${dockerRun}"
                
                }
        
            } 
        }  
     }
}
