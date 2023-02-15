pipeline{
    agent any
   tools {
       maven'maven'
   }
    environment {
        dockerhub=credentials('mydockerhub')

    }
    stages{
        stage('clean')
        {
            steps{
                sh 'mvn clean'
            }
        }
       stage('build image')
        {

            steps{

                sh 'docker build -t sairam1/testsample:${GIT_COMMIT} . '
            }
        } 
        stage('pushing to dockerhub')
        {
            steps{

                sh '''
                echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin
                docker push sairam1/sairam:${GIT_COMMIT} 
                docker logout
                '''
            }
        }
       

    }
 }
