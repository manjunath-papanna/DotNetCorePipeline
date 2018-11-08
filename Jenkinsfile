pipeline {
  agent any
  environment {
    dotnet = 'C:/Program Files/dotnet/dotnet.exe'
  }
  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/manjunath-papanna/DotNetCorePipeline.git', branch: 'master'
      }
    }
    stage('Restore') {
      steps {
        bat "dotnet restore DotNetCorePipeline.sln"
      }
    }
    stage('Clean') {
      steps {
        bat 'dotnet clean DotNetCorePipeline.sln'
      }
    }
    stage('Build') {
      steps {
        bat 'dotnet build DotNetCorePipeline.sln'
      }
    }
    stage('Test') {
      steps {
	      bat returnStatus: true, script: 'dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=cobertura ./ConsoleAppTest/ConsoleAppTest.csproj /p:CoverletOutput=/TestResults/ --logger trx --no-build'
	      step([$class: 'MSTestPublisher', testResultsFile:'**/*.trx', failOnError: true, keepLongStdio: true])
	    }     
    }
    stage('Publish') {
      steps {
        bat 'dotnet publish ./ConsoleApp/ConsoleApp.csproj -c Release -o ./dist/'
      }
    }
  }
}