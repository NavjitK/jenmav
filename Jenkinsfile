pipeline {  
    environment { 
        registry = "shashikantvermaji/java"  
        registryCredential = 'docker'  
        dockerImage = '' 
    }
    agent any 
     stages {  
        stage('Cloning our Git') { 
             steps {  
                 
                checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '98a69b20-e09c-4c58-8654-69feb048011e', url: 'https://github.com/shashikantvermaji/test1.git']]])
            }
            
         } 
         
         
        stage('mvn clean') { 
             steps {  
                 sh 'mvn clean'
                    }
            
         } 
         stage('compile') { 
             steps {  
                 sh 'mvn compile'
                    }
            
         } 
          stage('cobertura') { 
             steps {  
                 sh 'mvn clean cobertura:cobertura'
                    }
           }
           
           stage('testNG_Reprt') { 
             steps {  
                 sh 'mvn site'
                    }
           }
         
         stage('war created') { 
             steps {  
                 sh 'mvn install'
                    }
            
         } 
         
          
         stage('Building our image') { 
             steps { 
                 script { 
                     dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                 }
             } 
         }
 
        stage('Deploy our image') { 
             steps { 
 
    script { 
                     docker.withRegistry( '', registryCredential ) { 
                         dockerImage.push() 
                     }
                 } 
             }
         } 
 
        stage('Cleaning up') { 
 
            steps { 
 
                sh "docker rmi $registry:$BUILD_NUMBER" 
 
            }
 
        } 
 
    }

}
