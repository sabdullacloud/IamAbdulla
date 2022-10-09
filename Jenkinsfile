// Jenkinsfile - Declarative Pipeline
pipeline {
    agent any
    stages {
        stage('SCM Checkout') {
            steps {
                git 'https://github.com/sabdullacloud/IamAbdulla.git'
            }
        }
        stage('Compile-Package') {
            steps {
                def mvnHome = tool name: 'maven3', type: 'maven'
                sh "${mvnHome}/bin/mvn clean package"
                sh 'mv target/myweb*.war target/newapp.war'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t eitdockerhub/myweb:0.0.2 .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerpass', variable: 'dockerPassword')]) {
                sh "docker login -u eitdockerhub -p ${dockerPassword}"
                }
                sh 'docker push eitdockerhub/myweb:0.0.2'
            }
        }
        stage('Remove Previous Container') {
            steps {
                try{
                sh 'docker rm -f tomcattest'
                }catch(error){
                // do nothing if there is an exception
                }
            }
        }
        stage('Run Docker Deployment') {
            steps {
                sh 'docker run -d -p 8090:8080 --name tomcattest eitdockerhub/myweb:0.0.2'
            }
        }
    }
}

//If Jenkins and Apache Tomcat(Application server) running in same server 
    // [ Need to install ---> httpd, java, jenkins, docker, mysql(if need) ]
// Optional Code Review/Analysis ---> Sonarqube running in another server 
    // [ Need to install java, sonarqube-setup, mysql ]
// Optional Nexus Artifactory for distribute repo to required env ---> Nexus running in another server (java)
    // [ Need to install java and Add Docker Daemon file in Docker server for adding Nexus Repo Url's]
//Ref-link:-
//https://www.sonarqube.org/
//https://codebucketpipelien.s3.ap-southeast-1.amazonaws.com/sonarqube.docx
//https://techexpert.tips/sonarqube/sonarqube-installation-on-the-cloud-aws-ec2/
//https://help.sonatype.com/repomanager3/product-information/download
//https://www.fosstechnix.com/how-to-install-nexus-repository-on-ubuntu/
//https://www.howtoforge.com/how-to-install-and-configure-nexus-repository-manager-on-ubuntu-20-04/
//https://qiita.com/leechungkyu/items/86cad0396cf95b3b6973
//https://blog.sonatype.com/using-nexus-3-as-your-repository-part-3-docker-images