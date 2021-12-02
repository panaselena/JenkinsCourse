


pipeline {
   agent { node { label 'slave01' } }


    stages {
stage('Clone  Sources') {
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
        
        steps {
           sh '''
           
            chmod -R 777 $WORKSPACE/
            a=$WORKSPACE/../results/
	    if  ( env.LANGUAGE == 'Python');
		then

            		python3 /home/lena/workspace/script/test.py > $a/result_pth.txt
           
           
            		#python3 test.py >> $a/result_pth.txt

                
            fi           
                                                  '''
            
         }
            
      }
      
    
     stage ('Bash') {
       
    
        steps {
           sh '''
           
            
            chmod -R 777 $WORKSPACE/
            b=$WORKSPACE/../results/
            if  ( env.LANGUAGE == 'Bash');
		then

            cd /home/lena/workspace/script/
           
            ./test.sh > $b/result_bsh.txt
                
                       
            fi                                      '''
            
         }
            
      } 
  
  stage ('C') {
      
        steps {
           sh '''
           
           

            chmod -R 777 $WORKSPACE/
            d=$WORKSPACE/../results/

	    if  ( env.LANGUAGE == 'Bash');
		then

            echo "I'm sorry, I didn't learn the language 'С'" >> d/result_c.txt
           
                         
             fi          
                                                  '''
            
         }
            
      } 
                      
  	  
      
     stage ('All') {
       
        steps {
           sh '''
           
            
            chmod -R 777 $WORKSPACE/
            c=$WORKSPACE/../results/

		if  ( env.LANGUAGE == 'All');
			then

           	 cd /home/lena/workspace/script/
           
          	  ./test.sh >> $c/result_all.txt
            	  python3 /home/lena/workspace/script/test.py >> $c/result_all.txt
	          echo "I'm sorry, I didn't learn the language 'С'" >> $c/result_all.txt

		fi
                
    
                    
        
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
      cd /home/lena/workspace/
          cat ./results/*.* >> ${report_file}

      echo "#############################" >> ${report_file}
        '''
     }
  }                                              
}
