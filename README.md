pipeline {
   agent { node { label 'slave01' } }
    
    
    stages {
    stage('Clone  Sources') {
        steps {
          checkout scm
        } 
      }
        
        
       stage ('Choice  Language') {
            steps {
            
              echo "You choose ${LANGUAGE}"
            
            }   
        
      
        }
        stage ('Python') {
            when {expression { LANGUAGE == 'Python' }
            }
        
            steps {
               sh '''
               
                chmod -R 777 $WORKSPACE/
                a=$WORKSPACE/
                python3 /home/lena/workspace/script/test.py >> $a/result_pth.txt
               
               
                #python3 test.py >> $a/result_pth.txt
                    
                           
                                                      '''
                
             }
                
          }
          
        
         stage ('Bash') {
            when {expression { LANGUAGE == 'Bash' }
            }
        
            steps {
               sh '''
               
                
                chmod -R 777 $WORKSPACE/
                b=$WORKSPACE/
                cd /home/lena/workspace/script/
               
                ./test.sh >> $b/result_bsh.txt
                    
                           
                                                      '''
                
             }
                
          } 
          
         stage ('All') {
            when {expression { LANGUAGE == 'All' }
            }
        
            steps {
               sh '''
               
                
                chmod -R 777 $WORKSPACE/
                c=$WORKSPACE/
                cd /home/lena/workspace/script/
               
                ./test.sh >> $c/result_all.txt
                python3 /home/lena/workspace/script/test.py >> $c/result_all.txt
                    
        
                        
            
                                                      '''
                                                      
            }
         }    
                                                      
    }
}


