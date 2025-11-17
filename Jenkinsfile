node {
    // Match these with Global Tool Configuration names
    env.MAVEN_HOME = tool name: 'Maven3', type: 'hudson.tasks.Maven$MavenInstallation'
    env.JAVA_HOME  = tool name: 'JDK11', type: 'jdk'
    env.PATH = "${env.MAVEN_HOME}/bin:${env.JAVA_HOME}/bin:${env.PATH}"

    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        try {
            sh "${env.MAVEN_HOME}/bin/mvn -B -DskipTests=false clean package"
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
