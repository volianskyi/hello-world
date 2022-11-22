pipeline {
    agent {
        label 'java/1.8-docker-ecr'
    }
    tools {
        maven 'maven-3.6.3'
    }
    stages {
        stage('Build') {
            steps {
               sh 'mvn clean package'
            }
        }
    }
    post {
        success {
            setBuildStatus("Build succeeded", "SUCCESS");
        }
        failure {
            setBuildStatus("Build failed", "FAILURE");
        }
        cleanup {
            cleanWs()
        }
    }
}

void setBuildStatus(String message, String state) {
    step([
        $class            : "GitHubCommitStatusSetter",
        reposSource       : [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/volianskyi/hello-world.git"],
        contextSource     : [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
        errorHandlers     : [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
        statusResultSource: [$class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]]]
    ]);
}