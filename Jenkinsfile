pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'compiling the code....'
        sh 'mvn compile'
      }
    }

    stage('test') {
      steps {
        echo 'running unit test.....'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      steps {
      def branchName = sh( returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD' ).trim()
      if(branchName == "master") {
        parallel {
        stage('package') {
          steps {
            echo 'generating artifacts....'
            sh 'mvn package -DskipTests'
            archiveArtifacts 'target/*.war'
          }
        }

        stage('') {
          steps {
            sleep 1
          }
        }

      }
      }
      }
      
    }

  }
  tools {
    maven 'Maven 3.6.3'
  }
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
}
