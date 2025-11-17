// Jenkinsfile (Windows-friendly scripted pipeline)
node {
  // these names must match your Global Tool Configuration
  env.MAVEN_HOME = tool name: 'Maven3', type: 'hudson.tasks.Maven$MavenInstallation'
  env.JAVA_HOME  = tool name: 'JDK11', type: 'jdk'

  stage('Checkout') {
    checkout scm
  }

  stage('Build') {
    try {
      // Use Windows batch and call mvn.cmd via MAVEN_HOME
      bat "\"%MAVEN_HOME%\\bin\\mvn.cmd\" -B -DskipTests=false clean package"
    } catch (e) {
      currentBuild.result = 'FAILURE'
      throw e
    }
  }

  stage('Tests') {
    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
  }

  stage('Archive') {
    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
  }
}
