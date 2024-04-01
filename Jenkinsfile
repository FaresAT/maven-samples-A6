pipeline {
  agent any
  tools {
     maven 'maven 3.6.1'
     jdk 'test'
  }

  stages {
    stage('Grab Repo') {
      steps {
        git(url: 'https://github.com/FaresAT/maven-samples-A6.git', branch: 'master')
      }
    }
    
    stage('Bisect') {
      steps {
        sh 'git bisect start 198644632661c67b6c32f59e9047c11a70685e15 98ac319c0cff47b4d39a1a7b61b4e195cfa231e5'
      }
    }
    
    stage('Cleanup') {
      steps {
        sh 'git bisect run mvn clean test'
        sh "git bisect reset"
      }
    }
  }
}
