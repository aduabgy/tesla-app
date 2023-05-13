pipeline{
  agent any 
  tools {
    maven "maven 3.8.6"
  }  
  stages {
    stage('1GetCode'){
      steps{
        sh "echo 'cloning the latest application version' "
        git credentialsId: 'git-credentials', url: 'https://github.com/aduabgy/tesla-app'
      }
    }
  stage('3Test+Build'){
      steps{
        sh "echo 'running JUnit-test-cases' "
        sh "echo 'testing must passed to create artifacts ' "
        sh "mvn clean package"
      }
    }
    
    stage('4CodeQuality'){
      steps{
        sh "echo 'Perfoming CodeQualityAnalysis' "
        sh "mvn sonar:sonar"
      }
    }
    
    stage('5uploadNexus'){
      steps{
        sh "mvn deploy"
      }
    } 
   
    stage('8deploy2prod'){
      steps{
          deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://13.42.102.95:8080')], contextPath: null, war: 'target/*war'
      }
    }
}
 
  post{
    always{
      emailext body: 'we are good ', recipientProviders: [buildUser(), culprits()], subject: 'success', to: 'aduabgy@yahoo.com'
    }
    success{
      emailext body: '''Hey Korie 
Good Job.

Thanks 
Dad''', recipientProviders: [buildUser(), requestor(), upstreamDevelopers()], subject: 'success', to: 'aaas@gmail.com'
    } 
    failure{
      emailext body: '''Hey guys
Build failed. Please resolve issues.

Thanks
Landmark 
+1 437 215 2483''', recipientProviders: [buildUser(), developers()], subject: 'success', to: 'paypal-team@gmail.com'
    }
  } 
 
}
