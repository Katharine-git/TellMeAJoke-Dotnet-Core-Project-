pipeline {
  agent any

  environment {
        EMAIL_TO = 'katharinesheen@gmail.com'
    }

  stages {
    stage("Prepare") {
      steps {
        echo "Notify the build status";
        
      }
    }

    stage('Checkout') {
      steps {
        echo "Checking out from Git Repo";
    
       git url: 'https://github.com/Katharine-git/TellMeAJoke-Dotnet-Core-Project-.git',
        branch: 'master'
      }
    } 
    
    stage('Restore packages') {
      steps {
        bat "dotnet restore TellMeAJoke.csproj"
      }
    }



    stage('Build') {
      steps {
        bat "dotnet build   TellMeAJoke.csproj "
        
      }
    }

    stage('Publish') {
      steps {
        bat "dotnet publish  TellMeAJoke.csproj"
      }
    }

    stage ('Deploy')
    {
    steps{

       powershell """ 
   
                      Import-Module  WebAdministration
                      
                      Remove-WebSite -Name dotnetcoredeploy
                      Remove-WebAppPool -Name dotnetcoredeploy

                      New-WebAppPool -Name dotnetcoredeploy -Force

                      New-Website -Name dotnetcoredeploy -Port 82 -IPAddress * -HostHeader dotnetcoredeploy.com -PhysicalPath C:\\Users\\Administrator\\Documents\\DotNetApp\\Project_1\\TellMeAJoke\\bin\\Debug\\netcoreapp3.1\\publish -ApplicationPool dotnetcoredeploy -Force

                      New-WebBinding -Name dotnetcoredeploy -IPAddress "*" -Port 82 -Protocol http
                      
                       """

      }

    }
  }

  post {
      success {
          emailext body: 'Check console output at $BUILD_URL to view the results. ', 
                  to: "${EMAIL_TO}", 
                  subject: 'Build succeded in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
      }
      failure {
          emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                  to: "${EMAIL_TO}", 
                  subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
      }
  
  }
     

}






