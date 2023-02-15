pipeline{
    agent any
   tools {
       maven 'mymaven'
       jdk 'myjava'
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


        stage("Testing the application")
        {
            when
            {
                branch "test" 
            }
            steps
            {
                sh 'mvn test'          
            }
        }


        stage('pack')
        {
            when{
                branch "prod"
                }
            steps{

                sh 'mvn package'


            }
        }
       stage('build image')
        {
            when{
                branch "prod"
                }
            steps{

                sh 'docker build -t sairam1/sairam:${GIT_COMMIT} . '
            }
        } 
        stage('pushing to dockerhub')
        {
            when{
                branch "prod"
                }
            steps{

                sh '''
                echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin
                docker push sairam1/sairam:${GIT_COMMIT} 
                docker logout
                '''
            }
        }
       
         stage('Deploy App') {

             when{
                branch "prod"
                }

      steps {
           
        kubernetesDeploy configs: '**/appDeployment.yaml', kubeConfig: [path: ''], kubeconfigId: 'kube', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']

             }
        }
        
    }
 }
