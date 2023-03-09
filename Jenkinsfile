pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Clone'){
            
            steps{
                
                script{
                    
                    git branch: 'main', credentialsId: 'git_credentials', url: 'https://github.com/riyaz1121/task-09.git'
                }
            }
        }
    stage('Maven Build'){
        
        steps{
            
            script{
                def mavenHome = tool name: "Maven-3.9.0", type: "maven"
                def mavenCMD = "${mavenHome}/bin/mvn"
                sh "${mavenCMD} clean package"
            }
        }
      }
    stage('SonarQube analysis'){
        
        steps{
                script{
                     withSonarQubeEnv(credentialsId: 'sonar-token') {
                     def mavenHome = tool name: "Maven-3.9.0", type: "maven"
                     def mavenCMD = "${mavenHome}/bin/mvn"
                     sh "${mavenCMD} sonar:sonar"
                    } 
                }
           }
    }
       stage('upload war to nexus'){ 
           
           steps{
                script{
                    
                   nexusArtifactUploader artifacts: [
                       [
                           artifactId: 'shaik-1', 
                           classifier: '', 
                           file: 'target/shaik-1-3.9.9.jar', 
                           type: 'jar'
                           ]
                           ], 
                           credentialsId: 'nexus-credentials', 
                           groupId: 'shaik', 
                           nexusUrl: '3.95.83.239:8081/', 
                           nexusVersion: 'nexus3', 
                           protocol: 'http', 
                           repository: 'riyaz-release', 
                           version: '3.9.9'
               } 
           }
        }
        
    }
}
