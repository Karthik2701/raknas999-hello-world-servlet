pipeline {
    agent any 
    tools { 
        maven 'Maven' 
      
    }
stages { 
     
 stage('Preparation') { 
     steps {
// for display purposes

      // Get some code from a GitHub repository

      git 'https://github.com/raknas999/hello-world-servlet.git'

      // Get the Maven tool.
     
 // ** NOTE: This 'M3' Maven tool must be configured
 
     // **       in the global configuration.   
     }
   }

   stage('Build') {
       steps {
       // Run the maven build

      //if (isUnix()) {
         sh 'mvn -Dmaven.test.failure.ignore=true install'
      //} 
      //else {
      //   bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
       }
//}
   }
 
  stage('Results') {
      steps {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.war'
      }
 }
 stage('Sonarqube') {
    environment {
        scannerHome = tool 'SonarQube'
    }
    steps {
        withSonarQubeEnv('SonarQube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
  //      timeout(time: 10, unit: 'MINUTES') {
 //           waitForQualityGate abortPipeline: true
  //      }
    }
}
     stage('Artifact upload') {
      steps {
     nexusPublisher nexusInstanceId: '12345', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/var/lib/jenkins/workspace/HelloWorldServlet-Pipeline/target/helloworld.war']], mavenCoordinate: [artifactId: 'hello-world-servlet-example', groupId: 'com.geekcap.vmturbo', packaging: 'war', version: '$BUILD_NUMBER']]]
      }
 }
     stage('Deploy to container') {
        steps {
            deploy adapters: [tomcat9(credentialsId: 'Tomcat-cred', path: '', url: 'http://3.87.72.138:8080')], contextPath: 'HelloWorlServelt-Pipeline', war: '**/*.war'
}
     }
}
post {
        success {
            mail to:"raknas000@gmail.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Build success"
        }
        failure {
            mail to:"raknas000@gmail.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Build failed"
        }
    }       
}
