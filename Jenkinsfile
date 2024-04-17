pipeline {
  agent none
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'compiling the code....'
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'running unit test.....'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      // when {
      //  branch 'master'
      //}
      parallel {
        stage('package') {
          agent {
            docker {
              image 'maven:3.6.3-jdk-11-slim'
            }

          }
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

        stage('Build n Package') {
          agent any
          steps {
            script {
              docker.withRegistry('https://index.docker.io/v1/', 'dockercred') {
                def dockerImage = docker.build("xxxxxx/sysfoo:v${env.BUILD_ID}", "./")
                dockerImage.push()
                dockerImage.push("latest")
                dockerImage.push("dev")
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
