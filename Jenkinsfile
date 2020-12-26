node {
    stage('Preparation code') { // for display purposes
        // Get some code from a GitHub repository
        git 'https://github.com/kalyanamp/DevOps-CICD.git'
    }
    stage('Build') {
        sh 'mvn clean install package'
    }
    stage('post-build steps send war'){
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
    stage('Deployed on Tomcat server') {
        script {
          sshPublisher(
           continueOnError: false, failOnError: true,
           publishers: [
            sshPublisherDesc(
             configName: "anisble",
             verbose: true,
             transfers: [
              sshTransfer(
               sourceFiles: " ",
               removePrefix: " ",
               remoteDirectory: " ",
               execCommand: "ansible-playbook -i /tmp/hosts /tmp/copywarfile.yml"
              )
             ])
           ])
        }
    }
}
