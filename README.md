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
                a=$WORKSPACE/../results/
                python3 /home/lena/workspace/script/test.py > $a/result_pth.txt
               
               
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
                b=$WORKSPACE/../results/
                cd /home/lena/workspace/script/
               
                ./test.sh > $b/result_bsh.txt
                    
                           
                                                      '''
                
             }
                
          } 
          
         stage ('All') {
            when {expression { LANGUAGE == 'All' }
            }
        
            steps {
               sh '''
               
                
                chmod -R 777 $WORKSPACE/
                c=$WORKSPACE/../results/
                cd /home/lena/workspace/script/
               
                ./test.sh > $c/result_all.txt
                python3 /home/lena/workspace/script/test.py >> $c/result_all.txt
                    
        
                        
            
                                                      '''
                                                      
            }
         }    
        stage('Saving Results') {
         steps {
            echo 'Saving Results process..'
            sh '''
	      report_file="${HOME}/Documents/Deployment1/report"
              mkdir -p ${HOME}/Documents/Deployment1/              
              if [ -f "${report_file}" ]; then
                echo "file ${report_file} exists"
              else
	              touch ${report_file}
              fi
	      date >> ${report_file}
	      echo "USER=$USER JOB_NAME=$JOB_NAME" >> ${report_file}
              echo "Build Number $BUILD_NUMBER" >> ${report_file}
              cat "$WORKSPACE/../results/*.*" >> ${report_file}

	      echo "#############################" >> ${report_file}
            '''
         }
      }                                              
    }
}


