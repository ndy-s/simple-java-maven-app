node {
    try {
        stage('Build') {
            sh 'mvn -B -DskipTests clean package'
        }
        stage('Test') {
            sh 'mvn test'
        }
        stage('Manual Approval') {
            script {
                def userInput = input(
                    message: 'Lanjutkan ke tahap Deploy?',
                    ok: 'Proceed',
                    parameters: [booleanParam(defaultValue: true, description: 'Klik untuk melanjutkan', name: 'Proceed')]
                )
                if (!userInput) {
                    error("Pipeline dihentikan oleh pengguna.")
                }
            }
        }
        stage('Deploy') { 
            sh './jenkins/scripts/deliver.sh'
            sleep(time: 1, unit: 'MINUTES')
        }
    } catch (Exception e) {
        currentBuild.result = 'FAILURE'
        throw e
    } finally {
        stage('Post-build actions') {
            junit 'target/surefire-reports/*.xml'
        }
    }
}
