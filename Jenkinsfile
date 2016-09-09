node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/knolle74/ContDep_example.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
}
node {
   def mvnHome = tool 'maven'
   stage ('DEV Test')
   parallel 'Unit Tests': {
   //stage('Unit Tests') 
       //Unit Tests durchführen
      junit '**/target/surefire-reports/TEST-*.xml'
      //Ergebnisszusammenfassung anzeigen
      sh "more target/surefire-reports/test*.txt"
      archive 'target/*.jar'
   }, 'Findbugs': {
       sh "'${mvnHome}/bin/mvn' findbugs:findbugs"
   }
}

node {
   stage('Security PenTesting') {
       echo 'successfully finished with 0 Bugs and 2 warnings'
   }   
   stage('Deploy to DEV') {
       echo 'Deploy wird durchgeführt. Willkommen in DEV ...'
   }
   stage('BSM Script Testing') {
       echo 'successfully finished with no problem'
   }
   stage('Approval Step for QA Deploy') {
       // manuelle Bestätigung notwendig für zB Deploy nach PROD
       timeout(time:2, unit:'MINUTES') {
           input message:'Approve deployment?'
       }
   }
   stage('Deploy to QA') {
       echo 'Deploy wird durchgeführt. Und ab dafür ...'
   }
   stage('QA LAPT Testing') {
       echo 'Testing successfully finished. 1.000.000 Requests with 1 broken Response and meantime-response per request of 0.13 ms'
   }
   stage('Approval Step for Prod Deploy') {
       // manuelle Bestätigung notwendig für zB Deploy nach PROD
       timeout(time:2, unit:'MINUTES') {
           input message:'Approve deployment. Is final manual testing done?'
       }
   }
   stage('Create Change and wait for confirmation') {
       echo 'Change C09876543 wurde angelegt'
   }
   stage('Deploy to productional environment') {
       timeout(time:10, unit:'SECONDS') {
         echo 'Approval for Change C09876543 received. Deploying to prod ...'
       }
   }
   stage('Smoke Testing Production') {
       echo 'Tests passed. Welcome in real life'
   }
   
}
