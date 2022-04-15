pipeline {
  agent any

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
          sh 'ansible-playbook --private-key=${keyfile} -i hosts.ini playbook.yml'
        }
        // withCredentials([sshUserPrivateKey(credentialsId: "vagrant-private-key", keyFileVariable: 'keyfile')]) {
        //   sh 'ansible-playbook --private-key=${keyfile} -i hosts.ini playbook.yml'
        //     // sh 'scp -o "StrictHostKeyChecking=no" -i ${keyfile} ./sample vagrant@10.10.50.3:'
        // }
      }
    }

  }
}
