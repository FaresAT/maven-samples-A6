pipeline {
  agent any
  tools {
     maven 'maven 3.6.1'
     jdk 'test'
  }

  stages {
    // stage('Prepare Environment') {
    //   steps {
    //     script {
    //       env.REPO_URL = 'https://github.com/<YourGitHubUsername>/maven-samples-A6.git'
    //     }
    //   }
    // }

    stage('Grab Repo') {
      steps {
        git(url: 'https://github.com/FaresAT/maven-samples-A6.git', branch: 'master')
      }
    }
    
    stage('Checkout and Test Initial Commit') {
      steps {
        script {
          sh 'mvn clean test'
          try {
              sh '${MAVEN_HOME}/bin/mvn clean test'
          } catch (Exception e) {
              echo "Expected failure"
          }
        }
      }
    }
    
    stage('Git Bisect Setup') {
      steps {
        script {
          sh 'git bisect start'
          sh 'git bisect good 98ac319c0cff47b4d39a1a7b61b4e195cfa231e5'
          sh 'git bisect bad 198644632661c67b6c32f59e9047c11a70685e15'
        }
      }
    }
    
    stage('Automate Git Bisect') {
      steps {
        script {
          sh "git bisect run sh -c 'mvn clean test || exit 125'"
        }
      }
    }
    
    stage('Reset Bisect') {
      steps {
        script {
          sh 'git bisect reset'
        }
      }
    }
    stage('Cleanup') {
      steps {
        script {
          sh 'git checkout master'
        }
      }
    }
  }
}
