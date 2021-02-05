node{

    checkout scm
    // stage('GitHub Checkout'){
    //     git branch: 'master', credentialsId: 'git-creds', url: 'https://github.com/RMartinez99/devbops_user_microservice'
    //     // git credentialsId: '07225310-2f34-461a-a477-caa56d951f16', url: 'https://github.com/RMartinez99/devbops_user_microservice/'
    // }
    
    stage('User Test'){
        sh 'python3 test_User.py'
    }
    
    stage('Docker Build'){
        sh 'docker build -t rm267/devbops_user .'
    
    }

    stage('Push'){

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
            sh "ssh -o StrictHostKeyChecking=no ec2-user@100.25.17.1 ${dockerRm}"
            sh "ssh -o StrictHostKeyChecking=no ec2-user@100.25.17.1 ${dockerRmI}"
            sh "ssh -o StrictHostKeyChecking=no ec2-user@100.25.17.1 ${dockerRun}"
           
        }
    
    }   
}