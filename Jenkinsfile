pipeline {
  agent {
    docker {
      image 'maven:3.6.3-jdk-11-slim'
    }

  }
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
      when {
        branch 'master'
      }
      parallel {
        stage('package') {
          steps {
            echo 'generating artifacts....'
            sh 'mvn package -DskipTests'
            archiveArtifacts 'target/*.war'
          }
        }

        stage('error') {
          steps {
            sleep 1
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