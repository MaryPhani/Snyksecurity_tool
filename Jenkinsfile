pipeline{
    agent any
    environment {
        PATH = "/opt/apache-maven-3.6.3/bin:$PATH"
    }
    stages{
        stage('git clone'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'gitlab_cred', url: 'http://ec2-65-1-204-222.ap-south-1.compute.amazonaws.com/Ramakrishna/sonar-java-hello-world.git']]])
            }
        }
        stage('build'){
            steps{
               sh 'mvn clean install'
            }
        }
        /*stage('SonarQube Analytics') {
            steps {
                withSonarQubeEnv('sonarserver') {
                    sh 'mvn test jacoco:report sonar:sonar' 
                    
                    
                }
            }
        }*/
        stage('Snyk Test') {
            steps {
                echo 'Snyk Testing...'
                snykSecurity (
                    projectName: 'Snyk_security_tool', 
                    snykInstallation: 'snyk-latest', 
                    snykTokenId: 'snyk',
                    failOnIssues: false
                    
                )
            }
        }
    }
}
