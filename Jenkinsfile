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
    //We make an untash of the file
     unstash  'Packaged Files'
    //We create a File Object, depending if is Linux or Windows
    if (isUnix()){
        def dirPath='/home/jenkins/jenkins-loop'
    }
    else {
        def dirPath='c:/jenkins/jenkins-loop' 
    }
    def dir=new File(dirPath)
    // If the directory exists, delete it
    if (dir.exists()  && dir.isDirectory()  ){
        dir.deleteDir()
    }
    dir.mkdirs()  
   
    //I unzip and copy the files to the DEST dir
    sh "tar xvf files.tar" // bat "unzip files.tar"
    sh "cp *.jar ${dirPath}" //copy *.jar 
       
}