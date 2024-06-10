node{
    stage("git check out"){
        git 'https://github.com/Chandu1998/Insurance-Project.git'
    }
    stage("build and package"){
        sh "mvn clean package"
    }
    stage("creating docker image"){
        sh "docker build -t insureme-project ."
    }
    stage("pushing image to dockerhub"){
        withCredentials([string(credentialsId: 'dockerhubpass', variable: 'dockerpass')]){
            sh "docker login -u ckvk18 -p ${dockerpass}"
        }
    }
    stage("tagging and pushing image to dockerhub"){
        sh "docker tag insureme-project ckvk18/insureme-project:1"
        sh "docker push ckvk18/insureme-project:1"
    }
    stage("deploying on ansible"){
        ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultCredentialsId: 'dockerhubpass', vaultTmpPath: ''
    }
}
