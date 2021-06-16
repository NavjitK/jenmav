pipeline {  
    environment { 
        registry = "navjitkaur/jenmav"  
        registryCredential = 'docker'  
        dockerImage = '' 
    }
    agent any 
     stages {  
        stage('Cloning our Git') { 
             steps {  
                 
                checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '4ffd37ad-1445-4bfa-84f5-2ee9fcdc08ee', url: 'https://github.com/NavjitK/jenmav.git']]])
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
