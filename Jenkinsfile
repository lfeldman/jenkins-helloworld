pipeline {
    agent any
    
    stages {
        stage('Fetch dependencies') {
        /* This stage pulls the latest image from
           Dockerhub */
            steps {
                sh 'sudo docker pull karthequian/helloworld:latest'
          }
        }
        stage('Build docker image') {
        /* This stage builds the actual image; synonymous to
           docker build on the command line */
            steps {
            sh "sudo docker build . -t customapp:1"
            }    
        }
        stage('Test image') {
         /* This stage runs unit tests on the image; we are
            just running dummy tests here */
            steps {
                sh 'echo "Tests successful"'
          }
        }
        stage('Push image to OCIR') {
         /* Final stage of build; Push the 
            docker image to our OCI private Registry*/
        steps {
            
            sh "sudo docker login -u ociobenablement/identitycloudservice/lukasz.feldman@oracle.com -p 'ov;21h>xnnpEWVucXX.{' iad.ocir.io"
            /*
            sh "sudo echo 'ov;21h>xnnpEWVucXX.{' | docker login iad.ocir.io --username ociobenablement/identitycloudservice/lukasz.feldman@oracle.com --password-stdin"
            */
            sh "sudo docker tag customapp:1 iad.ocir.io/ociobenablement/customapp:custom"
            sh 'sudo docker push iad.ocir.io/ociobenablement/customapp:custom'
            
           }
         } 
         stage('Deploy to OKE') {
         /* Deploy the image to OKE*/

        steps {
            /*sh "'sudo cp /var/lib/jenkins/workspace/deploy.sh /var/lib/jenkins/workspace/jenkins-oci_master'"*/
            sh 'sh ../../hello-deploy.sh'
           }
         }     
    }
}
