properties([pipelineTriggers([githubPush()])])
node {
checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'netdevops-2', url: 'https://github.com/netdevops-2/dcautomation-2.git/']]])
stage('Build') {
    echo 'Build stage'
    sh "docker run -dit --name ansible_git netdevops/ansible_git_v1"
    sh 'docker exec -i ansible_git /bin/sh -c "git clone https://github.com/netdevops-2/dcautomation-2.git"'
}
stage('Test') {
    echo 'Test stage'
    sh 'docker exec -i ansible_git /bin/sh -c "ansible-playbook dcautomation-2/vlan.yml -i dcautomation-2/hosts --syntax-check"'
    sh 'docker exec -i ansible_git /bin/sh -c "ansible-playbook dcautomation-2/vlan.yml -i dcautomation-2/hosts --check"'
}        
stage('Clean up') {
    echo 'Clean up stage'
    sh "docker rm ansible_git -f"
}
}
