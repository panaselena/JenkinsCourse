pipeline {
   agent { node { label 'slave01' } }


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
        
        steps {
		script{
	if ("$LANGUAGE" == 'Python')
	{	
           sh '''
           
            
            a=$WORKSPACE/../results/
	    echo 'executing pth'
            python3 /home/lena/workspace/script/test.py > $a/result_pth.txt
	    echo 'pth done working'
	    '''
		}
			
		 else 
		  {
	sh '''echo 'Python not working' '''
           
	    }                                	               
          
         }
            
      }
  }    
    
     stage ('Bash') {
       
    
        steps {

	script {

	if ( "$LANGUAGE" == 'Bash' )
	{
           sh '''
           
            
            
            b=$WORKSPACE/../results/
           echo 'executing bsh'

            cd /home/lena/workspace/script/
           
            ./test.sh > $b/result_bsh.txt
		echo 'bsh done working'
				'''
		}
	    
	
                else
		{
		sh '''

		  echo 'Bash not working'
		'''
	    }

	}
     }
}                  
                                                 '''
             
  
 	 stage ('C') {
      
        steps {

	script {
	if ("$LANGUAGE" == 'C')
	{
           sh '''
            cd /home/lena/workspace/results
            echo "C" > /home/lena/workspace/results/result_c.txt
		'''
	}
	    
	    
           else{
		sh '''
		  echo 'C not working'
			'''
                        
                                                                                
         }
            
      } 
     }
}                 
  	  
      
     stage ('All') {
       
        steps {

	script {
	if ($LANGUAGE" == 'All')
	(
           sh '''
           
            
            chmod -R 777 $WORKSPACE/
            c=$WORKSPACE/../results/
	   	
		echo 'executing all=pth+bsh+C'

		
           	 cd /home/lena/workspace/script/
           
          	  ./test.sh >> $c/result_all.txt
            	  python3 /home/lena/workspace/script/test.py >> $c/result_all.txt
	          echo "I'm sorry, I didn't learn the language 'С'" >> $c/result_all.txt
		  echo 'all done working except C'
		  '''
		}
	
	  	  else
		{
		sh ''' echo 'All not working'

		'''                    
        
    		 }    
 	}
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
}



