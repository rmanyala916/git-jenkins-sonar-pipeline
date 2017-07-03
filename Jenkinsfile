stage 'CI'

node {
   checkout scm
   
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/rmanyala916/devopstoolkitproject.git'
      // Get the Maven tool.
      // ** NOTE:  This 'Maven' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'Maven'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean test"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean test/)
      }
   }

   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
   }
}


    stage('SonarQube analysis') { 
        node{
            def mvnHome
            mvnHome = tool 'Maven'
            withSonarQubeEnv('Sonar') { 

                if (isUnix()) {
                    sh "'${mvnHome}/bin/mvn' org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.603:sonar " + 
                    '-f pom.xml ' +
                    '-Dsonar.projectKey=org.sonarqube:java-sonar ' +
                    '-Dsonar.projectName=Java :: Simple Spring Project ' +
                    '-Dsonar.projectVersion=1.0 ' +
                    '-Dsonar.language=java ' +
                    '-Dsonar.sources=. ' +
                    '-Dsonar.tests=. ' +
                    '-Dsonar.test.inclusions=**/*Test*/** ' +
                    '-Dsonar.exclusions=**/*Test*/**'
                } else {
                    bat ("'${mvnHome}\bin\mvn' org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.603:sonar -f pom.xml " + "  -Dsonar.projectKey=org.sonarqube:java-sonar -Dsonar.projectName='Java :: Simple Spring Project'")


                }    
            }
        }
    }