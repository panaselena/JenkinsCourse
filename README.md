pipeline {
   agent any
    
    
    stages {
        stage('Clone Sources') {
    steps {
      checkout scm
      
       }
    }
        
    
       stage ('Choice Language') {
            steps {
            
              echo "You choose ${LANGUAGE}"
            
            }   
        
      
        }
        stage ('Python') {
            when {expression { LANGUAGE == 'Python' }
            }
        
            steps {
               sh '''
               
                mkdir Python_Lena
                chmod -R 777 $WORKSPACE/Python_Lena/
                a=$WORKSPACE/Python_Lena
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
               
                mkdir Bash_Lena
                chmod -R 777 $WORKSPACE/Bash_Lena/
                b=$WORKSPACE/Bash_Lena
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
               
                mkdir All_Lena
                chmod -R 777 $WORKSPACE/All_Lena/
                c=$WORKSPACE/All_Lena
                cd /home/lena/workspace/script/
               
                ./test.sh >> $c/result_all.txt
                python3 /home/lena/workspace/script/test.py >> $c/result_all.txt
                    
        
                        
            
                                                      '''
                                                      
            }
         }    
                                                      
    }
}
