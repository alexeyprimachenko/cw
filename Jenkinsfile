pipeline {
  environment {
    imagename = "alexeyprimachenko/cw"
    registryCredential = 'dockerhub_id'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Git') {
      steps {
        git([url: 'https://alexprimachenko@github.com/alexprimachenko/cw.git', branch: 'main'])
      }
    }
    stage('Make image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
              dockerImage.push('latest')
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
          sh "docker rmi $imagename:latest"
      }
    }
  }
}