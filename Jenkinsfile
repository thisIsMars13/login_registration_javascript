void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/thisIsMars13/login_registration_javascript.git"],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
} 

applicationIPAddress= "47.128.147.236"
sourceBranch = ghprbSourceBranch
targetBranch = ghprbTargetBranch

pipeline {
    agent any

    
    stages {
        stage('Deploy Sample Instance') {
            steps {
                script {
                    Integer port = 3000
                    String directory = "/var/www/login_registration_javascript"
                    String staging_env = "staging_env"
 
                    withCredentials([sshUserPrivateKey(credentialsId: "sshadmin", keyFileVariable: 'SSH_KEY')]) {
                        def remote = [
                            name: 'ubuntu',
                            port: 22,
                            allowAnyHosts: true,
                            host: "${applicationIPAddress}",
                            user: "ubuntu",
                            identityFile: SSH_KEY
                        ]

                        echo "Fetch branch and checkout to change branch"
                        sshCommand remote: remote, command: "cd ${directory} && sudo git fetch"
                        sshCommand remote: remote, command: "cd ${directory} && sudo forever stopall"
                        sshCommand remote: remote, command: "cd ${directory} && sudo git merge ${sourceBranch}"
                        sshCommand remote: remote, command: "cd ${directory} && sudo forever start app.js"
                    }
                     echo "This should be running"
                    echo "port is ${port}"
                    echo "directory is ${directory}"
                    echo "staging_env is ${staging_env}"
                
                    echo currentBuild.result
                }
            }
        }
    } 
}
