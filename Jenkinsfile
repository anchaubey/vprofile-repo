pipeline {
    agent any
    stages {
        stage('Git pull') {
            steps {
                git credentialsId: 'GITHUB_ID', url: 'https://github.com/anchaubey/vprofile-repo.git'
            }
                   }
        stage('Build Application') {
            steps {
                sh 'mvn -f pom.xml clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts...."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Deploy in Staging Environment'){
            steps{
                build job: 'deploy application to staging env'
 
            }
            
        }
        stage('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }
                build job: 'deploy application to prod env'
            }
        }
    }
}
