node ('dev') {
    
    git branch: 'main', url: 'https://github.com/ApasoftTraining/jenkins-script-bucle.git'
    
    // List of  files to ZIP
    def jarFiles = ['file1.jar', 'file2.jar', 'file3.jar', 'file4.jar', 'file5.jar']

    stage('PACK Files in files.tar') {
        try {
            // Loop through each file and deploy using a for loop         
            for (int i=0; i < jarFiles.size(); i++){
                def file=jarFiles[i]
                sh "tar rvf files.tar ${file}"
            }
               echo "Package finished"
            //Prepare the file for the next node
            stash includes: '*.tar', name: 'Packaged Files'
        } catch (Exception e) {
            //Control error
            echo "Packaged failed: ${e.getMessage()}"
            currentBuild.result='FAILURE'
            throw e
            
        }
    }
}

node('prod'){

    //We make an unstash of the file
    unstash 'Packaged Files'
    
    // If the directory exists, delete it // We create the directory
    def dir=""
    if (isUnix())
    {
        dir="/home/jenkins/jenkins-loop"
        sh "rm -rf ${dir}"
        sh "mkdir ${dir}"
        sh "tar xvf files.tar"
        sh "cp *.jar ${dir}"
    }
    else {
        dir="C:/jenkins/jenkins-loop" // c:\\jenkins\\
        bat "rmdir /Q /S ${dir} "
        bat "mkdir ${dir}"
        bat "unzip files.tar"
        bat "copy *.jar ${dir}"
    }
     
   
    //I unpack and copy the files to the DEST dir          
}