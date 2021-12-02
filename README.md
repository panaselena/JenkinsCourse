pipeline {
   agent { node { label 'slave01' } }
    
    
    stages {
    stage('Clone  Sources') {
        steps {
          checkout scm
        } 
      }


  stages {
    
       stage ('Choice  Language') {
            steps {
            
              echo "You choose ${LANG}"
            
            }   
        
        }
        stage ('Eng') {
            
            
         steps {
            sh '''
            
            if ( env.LANG == 'Eng');
             then
              echo 'I only execute'
             fi
            
            
            cd /home/lena/Misha/
               
            echo "Privet" >> Choose
            printenv
               
               '''
               
             }
         
        
        }
      
    }
 }
}
