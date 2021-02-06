pipeline {
     agent any
     stages {
         stage('GitHub Checkout'){
             steps{
                git credentialsId: 'git-creds', url: 'https://github.com/RMartinez99/devbops_user_microservice'
                
             }
         }
         stage('Prepping to Run Tests'){
            steps {
                sh 'curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py'
                sh 'python3 get-pip.py'
                sh 'pip3 install flask'
                sh 'pip3 install boto3'
                sh 'pip3 install requests'
                sh 'pip3 install bcrypt'
            }
         }
         stage('Making Sure the parts work') {
             steps {
                //  withEnv(["HOME=${env.WORKSPACE}"]) {
                sh 'python3 test_User.py'
                //  }
             }
         }
         stage('Loading onto Docker'){
             steps{
                 sh 'docker build -t rm267/devbops_user .'

             }

         }
        stage('Sailing off to Docker...'){
            steps{
                // withCredentials([string(variable: 'dockerHubPwd')]) {
                    sh "docker login -u rm267 -p ${dockerHubPwd}"
                // }
        
            
                sh 'docker push rm267/devbops_user'
            }
        }
        stage('Container Execution, on private EC2'){
            steps{

                
                // sh "ssh -o StrictHostKeyChecking=no ec2-user@34.202.164.220 ${dockerRm}"
                // sh "ssh -o StrictHostKeyChecking=no ec2-user@34.202.164.220 ${dockerRmI}"
                // sh "ssh -o StrictHostKeyChecking=no ec2-user@34.202.164.220 ${dockerRun}"
                sh "${dockerRm}"
                sh "${dockerRmI}"
                sh "${dockerRun}"
                
                
        
            } 
        }  
     }
}
