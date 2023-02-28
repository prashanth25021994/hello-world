pipeline {
environment {
imagename = ""
registryCredential = ''
dockerImage = ''
}
agent any
  tools {
        maven 'Maven 3.8.1'
        
        }  
stages {
stage('Cloning Git') {
steps {
git credentialsId: 'github', url: 'https://github.com/eashu/hello-world.git'
}
}

stage('Build') {
steps{
sh 'mvn -Dmaven.test.failure.ignore=true install'  
}
}
 
stage('Build the image') {
steps{
sh 'sudo docker build -t tunagar/helloworld:v1 Dockerfile  
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
