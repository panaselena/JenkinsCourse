pipeline {
   agent { node { label 'slave01' } }

    stages {
stage('Clone Sources') {
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
        
        steps {
		script{
	if ("$LANGUAGE" == 'Python'||"$LANGUAGE" == 'All' )
	{	
           sh '''
                       
            a=$WORKSPACE/../results/
	    echo 'executing  pth'
            python3 $a/../script/test.py > $a/result_pth.txt
	    echo 'pth done working'
	    '''
		}
			
		                
            }
          
      }
  }    
    
     stage ('Bash') {
       
        steps {

	script {

	if ( "$LANGUAGE" == 'Bash'||"$LANGUAGE" == 'All' )
	{
           sh '''
           
                         
            b=$WORKSPACE/../results/
           echo 'executing bsh'

            cd $b/../script
           
            ./test.sh > $b/result_bsh.txt
		echo 'bsh done working'
				'''
		}
	    
	
           }
     }
}                  
                                                 
               
 	 stage ('C') {
      
        steps {

	script {
	if ("$LANGUAGE" == 'C' || "$LANGUAGE" == 'All')
	{
           sh '''
            
            echo 'C' > /home/lena/workspace/results/result_c.txt
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



