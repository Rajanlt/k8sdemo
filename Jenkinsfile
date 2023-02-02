pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn -f hello-app/pom.xml -B -DskipTests clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts....."
                    archiveArtifacts artifacts: '**/*.jar'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -f hello-app/pom.xml test'
            }
            post {
                always {
                    junit 'hello-app/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Sonarqube') {
    
            steps {
                withSonarQubeEnv('SonarQube') {
                sh "/opt/sonar-scanner/bin"
        }
      }
  }
       stage('Upload Binaries to Nexus Artifactory')
         {
           steps 
              {
           nexusArtifactUploader artifacts: [[artifactId: 'demo-0.0.1-SNAPSHOT', classifier: 'snapshot', file: '', type: 'var/lib/jenkins/workspace/Test_pipeline/target/demo-0.0.1-SNAPSHOT.jar']], credentialsId: 'nexus_id', groupId: 'nexus', nexusUrl: '13.233.166.229:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'http://13.233.166.229:8081/repository/snapshot-artifact/', version: 'nexusid'       
        }
      }
    }
}
