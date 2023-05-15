node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'M3';
    withSonarQubeEnv() {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.qualitygate.wait=true -Dsonar.projectKey=SimpleMavenProject-1 -Dsonar.projectName='SimpleMavenProject-1'"
    }
  }
}


