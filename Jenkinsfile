pipeline {
  agent any

  parameters {
    choice choices: ['qa', 'production','staging'], description: 'Select environment for deployment', name: 'DEPLOY_TO'
  }


  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'sample', fingerprintArtifacts: true,
          projectName: 'sample', selector: lastSuccessful()
      }
    }
    stage('Deliver') {
      steps {
        sshagent(['vagrant-private-key']) {
          sh 'ansible-playbook -i ${DEPLOY_TO}.ini playbook.yml'
        }
        // withCredentials([sshUserPrivateKey(credentialsId: "vagrant-private-key", keyFileVariable: 'keyfile')]) {
        //   sh 'ansible-playbook --private-key=${keyfile} -i hosts.ini playbook.yml'
        //     // sh 'scp -o "StrictHostKeyChecking=no" -i ${keyfile} ./sample vagrant@10.10.50.3:'
        // }
      }
    }
    // stage('Test deployment with Newman') {
    //   steps {
    //     sh 'docker run -t postman/newman run https://www.getpostman.com/collections/ceccdf2a6442a8752f54'
    //   }
    // }
  }
}
