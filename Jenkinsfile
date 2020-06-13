pipeline {
 agent any
 environment {
  dotnet = 'D:\Gitrepo\Dotnet'
 }
 stages {
  stage('Checkout') {
   steps {
    git credentialsId: 'userId', url: 'https://github.com/NeelBhatt/SampleCliApp', branch: 'master'
    checkout([$class: 'GitSCM', branches: [[name: "refs/tags/${params.envname}"]], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'firstpipeline']], submoduleCfg: [], userRemoteConfigs: [[url: 'git@github.com:NeelBhatt/SampleCliApp.git']]])
   }
  }
  stage('Restore PACKAGES') {
   steps {
    bat "dotnet restore --configfile NuGet.Config"
   }
  }
  stage('Clean') {
   steps {
    bat 'dotnet clean'
   }
  }
  stage('Build') {
   steps {
    bat 'dotnet build --configuration Release'
   }
  }
  stage('Pack') {
   steps {
    bat 'dotnet pack --no-build --output nupkgs'
   }
  }
  stage('Publish') {
   steps {
    bat "dotnet nuget push **\\nupkgs\\*.nupkg -k yourApiKey -s            http://localhost/artifactory/api/nuget/nuget-internal-stable/com/sample"
   }
  }
 }
}
