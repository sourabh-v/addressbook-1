node {
   def mvnHome
   def version
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/sachingupta771/addressbook.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'LOCAL_MAVEN'
      version='2.3.5'
   }
   stage('UNITTEST') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore test -Pfunctional-test -DSkipUTs=true -DskipTests=true"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   
   stage('COVERAGE') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' pmd:pmd"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   
   stage('REVIEW') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' cobertura:cobertura"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('Publish') {
     nexusPublisher nexusInstanceId: 'NEXUS', nexusRepositoryId: 'RELEASES', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'addressbook_main/target/addressbook.war']], mavenCoordinate: [artifactId: 'addressbook_main', groupId: 'com.edurekademo.tutorial', packaging: 'war', version: '2.3.0']]]
   }
stage('Publish Reports') {
junit allowEmptyResults: true, testResults: '/demo/addressbook/addressbook_main/target/surefire-reports'
}

}
