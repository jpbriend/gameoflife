node('maven-jdk-8') {
    stage('Checkout Code') {
        checkout scm
    }
    withEnv(["PATH+MAVEN=${tool 'Maven 3.x'}/bin"]) {
        stage('Build') {
            sh "mvn clean compile"
        }
        stage('Unit Tests') {
            sh "mvn test"
        }
        stage('Packaging') {
            sh "mvn package"
        }
        stage('Integration Tests') {
            sh "mvn verify"
            stash includes: 'target/**', name: 'mvn-target'
        }
    }
}
stage('Waiting for approval') {
    timeout(time: 120, unit: 'SECONDS') {
        input 'OK to install ?'
    }
}
node('maven-jdk-8') {
    withEnv(["PATH+MAVEN=${tool 'Maven 3.x'}/bin"]) {
        stage('Install') {
            unstash 'mvn-target'
            sh "mvn install"
        }
    }
}