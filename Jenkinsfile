def pnodes = [:]

pnodes['run_on_master'] = {
    node('master') {
       def mvnHome
       stage('Source checkout') { // for display purposes
          git 'https://github.com/AdityaSP/devopsdemo'
          mvnHome = tool 'maven'
       }
       stage('Build') {
             bat(/"${mvnHome}\bin\mvn" install/)
        }
        stage('Archive'){
            archiveArtifacts allowEmptyArchive: true, artifacts: 'target/*.jar'
        }
    }
}

pnodes['run_on_localnode'] = {
    node('localnode') {
       def mvnHome
       stage('Source checkout') { // for display purposes
          git 'https://github.com/AdityaSP/devopsdemo'
          mvnHome = tool 'maven'
       }
       stage('Build') {
             bat(/"${mvnHome}\bin\mvn" install/)
        }
        stage('Archive'){
            archiveArtifacts allowEmptyArchive: true, artifacts: 'target/*.jar'
        }
    }
}

parallel pnodes

    node('master') {
        stage("Just echo"){
            emailext attachLog: true, body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - SUCCESS:
    
    Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'sp.aditya@gmail.com'
        }
}
