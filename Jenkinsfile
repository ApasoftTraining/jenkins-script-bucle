node ('dev') {
    
    
    // List of  files to ZIP
    def jarFiles = ['file1.jar', 'file2.jar', 'file3.jar', 'file4.jar', 'file5.jar']

    stage('PACK Files in files.tar') {
        try {
            // Loop through each file and deploy using a for loop
            for (int i = 0; i < jarFiles.size(); i++) {
                def file = jarFiles[i]
                echo "Packing ${file}..."
                tar rvf files.tar ${file}
            }           
            echo "All JAR files packaged successfully"
            //Prepare the file for the next node
            stash includes: '*.tar', name: 'Packaged Files'
        } catch (Exception e) {
            echo "Package failed: ${e.getMessage()}"
            currentBuild.result = 'FAILURE'
            throw e
        }
    }
}

node('prod'){
    //We make an untash of the file
    unstash 'Packaged Files' 
    
    //We create a File Object, depending if is Linux or Windfows
    if (isLinux(){
        def dirPath = '/home/jenkins/jenkins-loop'
    }
    else {
        def dirPath = 'c:/jenkins/jenkins-loop'
    }
     
    def dir= new File(dirPath)
    // If the directory exists, delete it
    if (dir.exists() && dir.isDirectory()) {
        println "The directory '${dirPath}' exists. Deleting it..."
        
        // Drop dierectory
        if (dir.deleteDir()) {
            println "The directory '${dirPath}' was successfully deleted."
        } else {
            println "Failed to delete the directory '${dirPath}'."
        }
    } else {
        // If it doesn't exist, create it
        println "The directory '${dirPath}' does not exist. Creating it..."
        
        if (dir.mkdirs()) {
            println "The directory '${dirPath}' was successfully created."
        } else {
            println "Failed to create the directory '${dirPath}'."
        }
    }
        //I unzip and copy the files to the DEST dir
        sh "tar xvf files.tar"
        sh "cp files.tar ${dir}"
}