pipeline {
  agent any
   stages {
    stage('Git checkout') {
      steps {
         echo 'This is for cloning the gitrepo'
         git branch: 'main', url: 'https://github.com/kavyaganji28/star-agile-insurance-project/'
                          }
            }
    stage('Create a Package') {
      steps {
         echo 'This will create a package using maven'
         sh 'mvn package'
                             }
            }

    stage('Publish the HTML Reports') {
      steps {
          publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/Insurance-job/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                        }
            }
    stage('Create a Docker image') {
      steps {
        sh 'docker build -t kavyaganji95/insureme:latest .'
                    }
            }
    stage('Login to Dockerhub') {
      steps {
      withCredentials([string(credentialsId: 'dockeruser', variable: 'dockervarcode')]) {
        // withCredentials([usernamePassword(credentialsId: 'dockeruser', passwordVariable: 'dockerpass', usernameVariable: 'username')]) {

        sh 'docker login -u kavyaganji95 -p ${dockervarcode}'
                                                                    }
                                }
            }
    stage('Push the Docker image') {
      steps {
        sh 'docker push kavyaganji95/insureme:latest'
                                }
            }
    stage('Ansible Playbook') {
      steps {
        ansiblePlaybook credentialsId: 'sshkey', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'deploy.yml', vaultTmpPath: ''
            }
        }

    }
}
