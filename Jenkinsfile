node{
    
    stage('GitHub Checkout'){
        // git branch: 'master', credentialsId: 'git-creds', url: 'https://github.com/RMartinez99/devbops_user_microservice'
        git 'https://github.com/RMartinez99/devbops_user_microservice/'
    }
    
    stage('DevBops Event Test'){
        sh 'python3 test_User.py'
    }
    
    stage('Docker Image Build'){
        sh 'docker build -t rm267/devbops_user .'
    
    }

    stage('Docker Image Push'){

        withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
            sh "docker login -u rm267 -p ${dockerHubPwd}"
    
            }
        
        sh 'docker push rm267/devbops_user'
    
    }

    stage('Run Docker Container on Private EC2'){
        def dockerRm = 'docker rm -f devbops_user'
        def dockerRmI = 'docker rmi rm267/devbops_user'
        def dockerRun = 'docker run -p 8092:80 -d --name devbops_event rm267/devbops_user'
        sshagent(['docker-server']) {
            sh "ssh -o StrictHostKeyChecking=no ec2-user@18.234.192.242 ${dockerRm}"
            sh "ssh -o StrictHostKeyChecking=no ec2-user@18.234.192.242 ${dockerRmI}"
            sh "ssh -o StrictHostKeyChecking=no ec2-user@18.234.192.242 ${dockerRun}"
           
        }
    
    }   
}