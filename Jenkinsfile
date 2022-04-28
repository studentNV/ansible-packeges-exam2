pipeline { 
    agent { label 'linux' }
    stages {
        stage('git_pull_rep_ansible') {
            steps {
                sh ' cd /opt/ansible-exam2  && git pull origin main '
            }
        }
        stage('deploy_web_app') {
            steps {
                echo '> Deploying web application.'
                ansiblePlaybook(
                    vaultCredentialsId: 'AnsibleVault',
                    inventory: '/opt/ansible-exam2/inventory.yaml',
                    playbook: '/opt/ansible-exam2/site.yaml'
                )
            }
        }
        stage('test_http') {
            steps {
                script {                   
                    try{
                        def sours = httpRequest 'http://192.168.100.72:8080/'
                    }
                    catch (exc) {
                        error('Status connection is ' + exc)
                    }
                }
            }
        }
    } 
}
