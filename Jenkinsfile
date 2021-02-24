pipeline {
    agent { label 'ltecomm'}
    triggers { pollSCM('* * * * *') }
    stages {
        stage ('scm') {
            steps {
                git branch : 'developer' , url : 'https://github.com/pramesh123/multibranch.git'
            }

        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage ('post build') {
            steps {
            archiveArtifacts artifacts: 'gameoflife-web/target/gameoflife.war', fingerprint: true
            stash name: 'warfile', includes: 'gameoflife-web/target/*.war'
        }
        }
        stage ('copy to other node') {
            agent {label  'ltelog'}
            steps {                
                unstash name: 'warfile'
            }
        }
        
        
    }
    post {
        always {
            mail to : 'rams.pattipaka@gmail.com',
            subject : "status of pipeline ${currentBuild.fullDisplayName}",
            body: "${env.BUILD_URL} has result ${currentBuild.result}" 
        }
    }


}