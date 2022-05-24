node {
    stage('Preparation code') { // for display purposes
        // Get some code from a GitHub repository
        git 'https://github.com/Yogeeswari-M/DevOps-CICD.git'
    }
    stage('Build') {
        sh 'mvn clean install package'
    }
    stage('Post-build steps send war'){
        script {
          sshPublisher(
           continueOnError: false, failOnError: true,
           publishers: [
            sshPublisherDesc(
             configName: "anisble",
             verbose: true,
             transfers: [
              sshTransfer(
               sourceFiles: "webapp/target/*.war",
               removePrefix: "webapp/target",
               remoteDirectory: " ",
               execCommand: "ls -ltr"
              )
             ])
           ])
        }
    }
    stage('Copy ansible-playbook to ansible server'){
        script {
          sshPublisher(
           continueOnError: false, failOnError: true,
           publishers: [
            sshPublisherDesc(
             configName: "anisble", // ansible server name 
             verbose: true,
             transfers: [
              sshTransfer(
               sourceFiles: "ansible/*",
               removePrefix: "ansible",
               remoteDirectory: " ",
               execCommand: "ls -ltr "
              )
             ])
           ])
        }
    }
    stage('Deployed on Tomcat server') {
        script {
          sshPublisher(
           continueOnError: false, failOnError: true,
           publishers: [
            sshPublisherDesc(
             configName: "anisble", // ansible server name 
             verbose: true,
             transfers: [
              sshTransfer(
               sourceFiles: " ",
               removePrefix: " ",
               remoteDirectory: " ",
               execCommand: "ansible-playbook -i hosts copywarfile.yml --extra-vars ansible_sudo_pass=red123"
              )
             ])
           ])
        }
    }
}
