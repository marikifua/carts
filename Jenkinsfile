node {
  def mvnHome
  stage('Preparation') { // for display purposes
     // Get some code from a GitHub repository
     git 'https://github.com/marikifua/carts.git'
     // Get the Maven tool.
     // ** NOTE: This 'M3' Maven tool must be configured
     // **       in the global configuration.
     mvnHome = tool 'M3'
  }
  stage('Build') {
     // Run the maven build
     withEnv(["MVN_HOME=$mvnHome"]) {
        if (isUnix()) {
           sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
        } else {
           bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
        }
     }
  }
  stage('TEST Results') {
     junit '**/target/surefire-reports/TEST-*.xml'
     archiveArtifacts 'target/*.jar'
  }
   
  stage('Deploy') {
     sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/carts/target/carts.jar jenkins@jenkins:~'
     sh 'ssh -o StrictHostKeyChecking=no jenkins@jenkins sudo systemctl restart carts'
       }
}
