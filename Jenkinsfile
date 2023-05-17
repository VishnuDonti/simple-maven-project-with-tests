node {
  try {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'M3';
    withSonarQubeEnv() {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.qualitygate.wait=true -Dsonar.projectKey=SimpleMavenProject-1 -Dsonar.projectName='SimpleMavenProject-1'"
    }
    
  }
  } catch (e) {
    echo 'This will run only if failed'
  setBuildStatus("Build Failed", "FAILED") 
        // Since we're catching the exception in order to report on it,
        // we need to re-throw it, to ensure that the build is marked as failed
    throw e
  } finally {
   def currentResult = currentBuild.result ?: 'SUCCESS'
        if (currentResult == 'UNSTABLE') {
            echo 'This will run only if the run was marked as unstable'
            setBuildStatus("Build Unstable", "FAILED") 
        } else {
          setBuildStatus("Build Stable", "SUCCESS") 
        }    
} 
}

void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/VishnuDonti/simple-maven-project-with-tests"],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}





