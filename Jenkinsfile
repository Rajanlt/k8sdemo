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
        stage('SonarQube Analysis') {
            steps {
            sh "mvn clean verify sonar:sonar -Dsonar.projectKey=test"
        }
      }
  }
       stage('Nexus artifact uploader') {
           steps {
           nexusArtifactUploader artifacts: [[artifactId: 'demo-0.0.1-SNAPSHOT', classifier: 'snapshot', file: '', type: 'var/lib/jenkins/workspace/Test_pipeline/target/demo-0.0.1-SNAPSHOT.jar']], credentialsId: 'Nexus_ID', groupId: 'Nexus_ID', nexusUrl: '13.234.38.191:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'http://13.233.166.229:8081/repository/snapshot-artifact/', version: 'nexusid'       
        }
      }
    }
