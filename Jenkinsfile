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
        git([url: 'https://alexeyprimachenko@github.com/alexeyprimachenko/cw.git', branch: 'main'])
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
    stage("Release") {
      steps {
        sh "ansible-playbook coursework.yml"
        sleep 60
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